+++
title = "msal4j With Groovy"
date = "2021-01-23"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["graph-api", "groovy", "groovy-programming", "java", "microsoft", "msal", "msal4j"]
tags = ["graph-api", "groovy", "groovy-programming", "java", "microsoft", "msal", "msal4j"]
+++

I've written before about the fact that I [use Groovy heavily](https://unusually.pink/groovy-programming-creating-an-iso-date-at-a-specific-date-and-time-in-utc/) in my current role since the platform I work the most frequently with is able to treat it as a first class citizen, whereas using something like PowerShell or Python adds a few more hurdles. The other thing I work with heavily is Office 365; this got me wondering if I might be able to combine the two by using Groovy to query [Microsoft's Graph API](https://developer.microsoft.com/en-us/graph/). Microsoft's current standard for interacting with the Graph API, replacing the old Active Directory Authentication Library (ADAL), is the Microsoft Authentication Library (MSAL.) [Microsoft provides MSAL packages for a _ton_ of popular languages](https://github.com/AzureAD), and considering that Groovy is a superset of Java, I figured that I should be able to leverage the [msal4j version](https://github.com/AzureAD/microsoft-authentication-library-for-java) within Groovy.

In my particular case, the code that I write is typically used as "glue" to link disparate systems together. As such, I based my work off of the Microsoft sample code for [authenticating as a daemon with a client secret](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/samples/confidential-client/ClientCredentialGrant.java). This is the method I've always used with my PowerShell and Python code. tl;dr, I was able to get this working with almost no difference from the Java sample, and you can see the end code that successfully authenticates and queries for the users in an AAD instance [on my GitHub account](https://github.com/jfabry-noc/GroovyMSAL/blob/main/graphToken.groovy).

As you can see if you check out the code, I first used [Grape for my dependency management](https://docs.groovy-lang.org/latest/html/documentation/grape.html). I'm admittedly not much of a Java developer, but I found Grape to be a very handy way to add additional libraries with very little overhead. It's a much more Python-esque experience than trying to add `.jar` files to a project in an IDE. There were a few dependencies I needed for this project, but the main one, msal4j, was added via:

```java
// https://mvnrepository.com/artifact/com.microsoft.azure/msal4j       
                
@Grapes(                     
    @Grab(group='com.microsoft.azure', module='msal4j', version='1.8.1')       
        )
```

To figure out what the heck to enter to import the library via Grape, I used the [Maven Repository](https://mvnrepository.com/), which gives you the additions required to use Grape -- or other dependency management tools like Maven or Gradle. Just be sure you're using the [msal4j library](https://mvnrepository.com/artifact/com.microsoft.azure/msal4j/1.8.1), which is designed for vanilla Java. The [library just named msal](https://mvnrepository.com/artifact/com.microsoft.identity.client/msal) is the one designed for Android. This was my first experience using Grape, and the only issue I ran into was a failure to realize that I needed to immediately include `import` statements after all of my Grape calls; adding the library doesn't magically add the functions and classes you may need, hence all of the msal-specific imports like:

```java
import com.microsoft.aad.msal4j.ClientCredentialFactory;
import com.microsoft.aad.msal4j.ClientCredentialParameters;          
import com.microsoft.aad.msal4j.ConfidentialClientApplication;
import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.IClientCredential;
import com.microsoft.aad.msal4j.MsalException;
import com.microsoft.aad.msal4j.SilentParameters;
```

Once I verified I could execute my baseline code making the imports without any issues, I started converting the function for getting an access token. Since this was completely new territory for me, I was running it against my free developer tenant. Anyone is free to sign up for the [Microsoft 365 Developer Program](https://developer.microsoft.com/en-us/microsoft-365/dev-program), which gets you a free tenant, 25 Office 365 E5 licenses, and plenty of other fun Azure goodies to test with. Be mindful that this is _not_ something for production purposes, and every 90 days Microsoft performs an evaluation on the tenant to verify that you're using it for development purposes and not for free access into Office 365. Admittedly, mine has gone neglected for many 90 day cycles without any active development work, but I also wasn't actively using the O365 accounts for anything, which has been cool in Microsoft's book so far. My tenant has always been renewed, and I was happy to have it around for testing this out. I [registered a new application in Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app), and then I tested the details from it that are typically needed for getting a token (the tenant ID, the client ID, and the client secret) in some existing Python code to verify that everything was working as expected.

With the foundation out of the way, I worked to [modify Microsoft's acquireToken() function](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/samples/confidential-client/ClientCredentialGrant.java) to be a bit more Groovy-esque:

```java
// Function to get an access token.
 def GetAccessToken(String clientId, String authority, String secret, String scope) {
     // Create the app.
     ConfidentialClientApplication app = ConfidentialClientApplication.builder(clientId, ClientCredentialFactory.createFromSecret(secret)).authority(authority).build()
     ClientCredentialParameters clientCredentialParam = ClientCredentialParameters.builder(Collections.singleton(scope)).build()
     def result = app.acquireToken(clientCredentialParam).join()

     // Return to the caller.
     result
 }
```

This worked, though I didn't realize it at first. I originally took my calling code of:

```java
def accessToken = GetAccessToken(aadConfig.client_id, aadConfig.authority, aadConfig.secret, aadConfig.scope)
```

And attempted to just print it to the screen:

```java
println accessToken
```

This resulted in an error message that spent quite a while driving me moderately insane:

> com.microsoft.aad.msal4j.MsalClientException: Cached JWT could not be parsed: Invalid JWT serialization: Missing dot delimiter(s)

Not being super familiar with the `IAuthenticationResult` object type, this threw me for a bit of a loop since I didn't really know what to expect. Searching for this error just turned up generic results surrounding the JWT library that Microsoft calls from within the msal4j library. Searching for it in relation to msal4j in particular returns almost no results, and searching for with respect to Groovy returns _literally_ no results.

I tried to make a function to query the Microsoft Graph using the `accessToken` value passed as the Bearer, but that simply resulted in the same error. Since I was working on this on my MacBook Pro running a [beta build of the OpenJDK 16 for Apple's M1 chip](https://formulae.brew.sh/formula/openjdk#default) and on a Linux VPS with OpenJDK 11, I tried running against OpenJDK 8 since that's the version that plays the nicest with Groovy, but I got the same result. I eventually even went to the point of setting up a Java environment with [IntelliJ IDEA](https://www.jetbrains.com/idea/) to verify that the sample code worked in vanilla Java 15, which it did. While looking at the Java code in IDEA, though, I realized that the sample output was _not_ calling the `accessToken` directly, but was rather calling a method on it:

```java
def tokenValue = accessToken.accessToken()
```

I modified my code with this, passing the value from the method to my `GetUsersListFromGraph()` function, and sure enough the user accounts from my test tenant were dumped to the screen as expected. Obviously, this error was a pretty simple mistake on my part to not leverage the object from msal4j properly. That being said, I figured that noting the error message might be helpful for someone else who makes the same mistake and finds the same lack of results when searching for the error message. I also thought it was quite slick how seamlessly Grape makes importing libraries into Groovy code without needing to worry about downloading `.jar` files, specifying a `classpath` when calling Groovy, etc. I think it'll be interesting to see what new integrations I can create now with our monitoring platform.
