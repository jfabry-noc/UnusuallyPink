+++
title = "Creating a JWT in the Groovy Programming Language"
date = "2020-07-12"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["groovy", "groovy-programming", "java", "json-web-token", "jwt", "programming"]
tags = ["groovy", "groovy-programming", "java", "json-web-token", "jwt", "programming"]
+++

On Friday I found myself in a new situation. I was working with APIs for a new service my company has started using, but their setup was a bit more involved than what I've typically experienced. Accessing many services via their REST API requires you to follow a few steps to generate an application ID and an API key, you pass those with your request, and you're done. The downside to this is that it can open up security vulnerabilities; if something happens to your API keys, for example, [they can be used for nefarious purposes](https://medium.com/@selvaganesh93/what-happens-if-you-accidentally-commit-your-aws-access-token-to-public-github-be50d378b4c7). Enter the [JSON Web Token](https://medium.com/@selvaganesh93/what-happens-if-you-accidentally-commit-your-aws-access-token-to-public-github-be50d378b4c7) or JWT. You create a JWT by parsing together a bunch of information, like your application ID, the validity period, etc., sign it with a secret, and exchange it for an access token you can actually use to make your normal API calls.

A JWT has a few advantages. First, it expires. The validity period varies based on the service; the one I was working with was 30 minutes. So if your access token is compromised, it can only work for however much time is left on the token. They can also be configured to leverage a UUID as a one-time [nonce](https://en.wikipedia.org/wiki/Cryptographic_nonce) to prevent replay attacks.

# Background

The new service I was working with used JWTs for authentication, so I had to figure out how to do that. The vendor provided sample code, but they were leveraging Python and using a library in Python to handle the JWT. That didn't help me too much because leveraging Python in my current setup would be difficult, and calling a library means the sample code didn't show me how to piece things together. I've mentioned before that the best language for the platform I'm dealing with at the moment is [Groovy](https://www.unusually.pink/blog/groovy-programming-creating-an-iso-date-at-a-specific-date-and-time-in-utc), but after some searches I found essentially no information on parsing together a JWT in Groovy. My other option for languages is PowerShell, so after some searches I found a [hero on Reddit who posted the exact code to create a JWT](https://www.reddit.com/r/PowerShell/comments/8bc3rb/generate_jwt_json_web_token_in_powershell/). I modified the code a little bit to account for the properties the service I was leveraging required in the payload; I managed to create a JWT, exchange it for an access token, and make successful requests from the API. Awesome!

Yesterday, though, I found myself sitting at home during another quarantine weekend, and I decided to see if I could recreate that code in Groovy since the PowerShell code was extremely readable; I just had to figure out how to do the same thing in Grooovy.

# High Level

At a high level, the process of creating a JWT looks like this.

1. Find the current time and the expiration time for the token, both in [Unix time](https://en.wikipedia.org/wiki/Unix_time).
2. Create a UUID to prevent replay attacks.
3. Create maps for the header and payload.
4. Convert those maps to JSON and then encode them as UTF-8 base64 strings.
5. Combine the encoded header and payload. Then create a SHA256 signature for it based on the secret key of the application where I generated the application ID.
6. Combine the header, payload, and signature together. Pass that to the service with HTTP POST, and receive back the authentication token.

# The Code

I'll paste individual snippets here, but the full code is in a [Github repository](https://github.com/JFFail/GroovyJWT). For reference with this post, my function definition looks like this; you can see that I'm passing a lot of information in for everything the payload will need:

def createJWT(JsonSlurper slurper, Integer validSeconds, String appID, String tenantID, String appSecret, String iss)

### Unix Time

Getting Unix time was pretty straightforward given the `currentTimeMillis` function in Java's System library.

```java
TimeZone.setDefault(TimeZone.getTimeZone('UTC'))
def rightNowMilli = System.currentTimeMillis()
def rightNowSec = Math.round(rightNowMilli / 1000)
def expirationSec = rightNowSec + validSeconds
```

The only hangup was that I need the time in seconds, not milliseconds, thus why I divided the value by 1000. After that, I just had to add on the number of seconds for the lifetime of the token to get the expiration time; in my case it was 1800 seconds (30 minutes.)

### UUID

Next up I needed to generate a UUID as a unique identifier so that no one can try to re-issue this exact same request. There's a UUID object type already, so I could generate a new, random UUID with:

```java
def jtiValue = UUID.randomUUID().toString()
```

### Maps

For ease of later conversion to JSON, I next created maps for both the header and the payload.

```java
Map header = [alg: "HS256", typ: "JWT"]
Map payload = [exp: expirationSec, iat: rightNowSec, iss: iss, sub: appID, tid: tenantID, jti: jtiValue]
```

I hard-coded the values in the header map, though it's worth mentioning SHA256 could be different. Likewise, the payload will depend heavily on the service from which a token is being requested; this is where you're most likely to need to make modifications specific to your use case.

### JSON Conversion

Next the maps are converted to JSON strings. Groovy's `JsonOutput` library makes this easy with a single method.

```java
def headerJson = JsonOutput.toJson(header)
def payloadJson = JsonOutput.toJson(payload)
```

Note that I needed to import the library before calling it.

```java
import groovy.json.JsonOutput
```

### Base64 Conversion

The JSON values for the header and payload both need to be converted to base64 strings. I initially started doing this in a **far** more difficult manner by creating a function that would convert the bytes and return a byte array before stumbling across the fact that Strings have a `getBytes` method. Note that Strings also have a `bytes` property I could call directly; in my testing this seemed to give me the same result, but I liked using `getBytes` instead because I could specify that they were UTF-8.

```java
def headerBase64 = headerJson.getBytes("UTF-8").encodeBase64().toString().split("=")[0].replaceAll("\\+", "-").replaceAll("/", "_")
def payloadBase64 = payloadJson.getBytes("UTF-8").encodeBase64().toString().split("=")\[0\].replaceAll("\\+", "-").replaceAll("/", "_")
```

I didn't see any cases where the `replaceAll` methods really did anything, but since they were in the original code I figured there must have been some use.

### Signature

This part basically involved a **lot** of [DuckDuckGo](https://duckduckgo.com) searches and piecing together things I found. Most of this is just Java code, and I'm honestly a little surprised that it worked... [this commit](https://github.com/JFFail/GroovyJWT/commit/c43d189d9bcab9ca81ca4d0df7820e4532ec3299#diff-d351cb51d1faad4d0047444e8ca114ac) shows how confident I was feeling. Note that once again I had to make some imports to leverage different crypto libraries:

```java
import javax.crypto.spec.SecretKeySpec
import javax.crypto.Mac
```

Then the code for it is:

```java
def toBeSigned = headerBase64 + "." + payloadBase64
SecretKeySpec secretKeySpec = new SecretKeySpec(appSecret.getBytes("UTF-8"), "HmacSHA256")
Mac mac = Mac.getInstance("HmacSHA256")
mac.init(secretKeySpec)
byte[] digest = mac.doFinal(toBeSigned.getBytes("UTF-8"))
def signature = digest.encodeBase64().toString().split("=")[0].replaceAll("\\+", "-").replaceAll("/", "_")
```

This is first concatenating together the header and payload, with a period separating the two base64 encoded values. Then I create a secret key specification using the secret key from my app. Next I instantiate a message authentication code using SHA256. That's used to create a signature against the aforementioned header/payload combination, the result of which is also a base64 encoded string.

### Combination

The final step is to simply concatenate together the header, payload, and signature, all separated by periods:

```java
def token = headerBase64 + "." + payloadBase64 + "." + signature
```

This value is what I return to the caller. I won't go over the details here, but in the GitHub repository I have code where I actually make a POST against the API endpoint and receive an access token back in exchange for the JWT I created, so I know everything is working. I _might_ update that code in the future to use a newer [HttpClient](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/HttpClient.html) from Java 11+ based on some things I had done last night in Kotlin; if I end up doing that I think it would be a good item for another post in the future.

Until next time, crypto on and stay pink!
