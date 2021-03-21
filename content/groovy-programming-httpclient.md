+++
title = "Groovy Programming HttpClient"
date = "2020-07-17"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["groovy", "httpclient", "httprequest", "java", "programming", "rest"]
tags = ["groovy", "httpclient", "httprequest", "java", "programming", "rest"]
+++

As a follow-up to my [post on creating a JWT in Groovy](https://www.unusually.pink/blog/creating-a-jwt-in-the-groovy-programming-language), I _did_ manage to figure out how to make an [HttpClient](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/HttpClient.html) in Groovy as opposed to making raw connections. You can see this implemented in the [GitHub repository](https://github.com/JFFail/GroovyJWT/blob/master/jwt.groovy) I used for the previously linked post. It was honestly pretty easy to do, and there are **tons** of tutorials out there; the code is essentially the same regardless of whether you're doing it in Java, Kotlin, or Groovy. Similar to the last time, it'll be easier to look at all of the code in the GitHub repo, but I'll call out the specific snippets I'm referencing throughout the post.

### Imports

I do need a handful of imports to get up and running with this:

```java
import java.net.http.HttpClient
import java.net.http.HttpRequest
import java.net.http.HttpRequest.BodyPublishers
import java.net.http.HttpResponse
import java.net.http.HttpResponse.BodyHandlers
```

If these fail to import when you try to execute the code, you are likely _not_ operating at Java 11 or above. More on why I know that later...

### Payload

In the last post I created a JWT. Now I need to take it and parse it into JSON to send it to the API endpoint so that I can get an access token. The simplest way to do this is to place it in a Map and then convert the Map to JSON.

```java
Map payloadMap = [auth_token: jwt]
def payloadJson = JsonOutput.toJson(payloadMap)
```

### Create The Client

The process of making requests in this method involves 3 steps. The first is to create the HttpClient object.

```java
def httpClient = HttpClient.newBuilder()
    .connectTimeout(Duration.ofSeconds(5))
    .followRedirects(HttpClient.Redirect.NORMAL)
    .build()
```

You can set any number that fits with your use case for the timeout duration; 5 seconds was safe for me. There are a _lot_ of other options for the client so be sure to check out the documentation if you need more.

### Create The Request

The request object is where the specifics of the connection are identified, like the URL and the method.

```java
def request = HttpRequest.newBuilder()
    .POST(HttpRequest.BodyPublishers.ofString(payloadJson))
    .uri(URI.create(url))
    .headers("Content-Type", "application/json; charset=utf-8", "Accept", "\*/\*")
    .build()
```

In the second line, I'm specifying POST since I need to send data. I'll cover a GET example later on. In the same line, I need to specify the format of my payload as being JSON; just throwing a JSON string at the endpoint will **not** work. If you only have a single header you can use `.header` instead of `.headers`. What caught me off guard with the headers is that you specify multiple of them not as key-value pairs like a Map but as a simple list, with each value just following its corresponding key.

### Send The Request

With the client and request both created, now it's time to send the request.

```java
def response = httpClient.send(request, HttpResponse.BodyHandlers.ofString())
```

This is also pretty straightforward; it's just worth mentioning that I'm specifying I want the response to be parsed into a string.

### Error Checking

I can check the `statusCode()` method to verify my request was successful and then take action upon the reply.

```java
if( response.statusCode() == 200 ) {
    println "${response.body()}"
} else {
    println "ERROR: Status code: ${response.statusCode()}"
}
```

From here I can parse the results to a Map and do my normal thing.

### GET Example

Using GET is the same as POST except for some details in the request. Obviously POST is replaced with GET, and then I have additional headers to specify.

```java
def request = HttpRequest.newBuilder()
    .GET()
    .uri(URI.create(currentUrl))
    .headers("Content-Type", "application/json; charset=utf-8", "Accept", "\*/\*", "Authorization", "Bearer $token")
    .build()
```

Naturally the exact headers you need will depend on the API you're calling. Other than that, though, the process is the exact same as it was with POST, including the way you execute the request.

Of course, after I implemented all of this I discovered that the back-end in my environment that was actually executing the code was _not_ running Java 11 or better, so I couldn't even use this setup. It's good to know for the future, though!
