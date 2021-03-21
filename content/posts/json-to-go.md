+++
title = "JSON-to-Go"
date = "2020-07-09"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["go", "golang", "json"]
tags = ["go", "golang", "json"]
+++

Lately I've been working through the _very_ arduous (for me) process of learning [Go](https://golang.org/) for some personal projects. I selected Go because I typically use interpretted, dynamically typed languages for work, so I thought it would be a good learning experience to work with a compiled, statically typed language. To me at least, Go seemed a bit more approachable than something like C or Rust. I started trying to learn [Kotlin](https://kotlinlang.org/) since I've been working with another JVM-based language in [Groovy](https://failti.me/posts/groovy_prog/), but it's extremely difficult to use Kotlin from just the command line without an IDE; when I couldn't figure out how to add an external package to a project without an IDE I basically gave up on it since it didn't fit at all into my workflow. Go, on the other hand, has a handy package manager built into the same binary you use to compile your own code.

After going through a book to get the basics of Go down (I won't link the book because I actually thought it was a pretty terrible source, and I wouldn't recommend it) I jumped in to doing a little bit of API work since that'll be important for some of the project ideas I'm kicking around. It was fairly simple to look up how to leverage the `io/ioutil` and `net/http` packages to make an unauthenticated call to a REST API endpoint. This gives the data back in a byte slice. I can cast that to a string to view it and verify that I got back the expected data, but obviously I can't actually _do_ anything I want with the data in a byte slice. In many other languages this is where I would use some type of JSON library to parse the response into something like a map/hashtable/dictionary. I'm used to languages where the interpreter just kind of figures that out for you based on the syntax of the JSON.

Go isn't like that, though. Instead, I need to define a `struct` that matches the format of the API response. If I have that, I can use the `encoding/json` package in Go to create a struct. That's something I _could_ do manually, but that would be extremely tedious. For example, this is what I see when dumping the byte array I get back from querying my [own Mastodon account](https://mastodon.sdf.org/@failtime) as a string:

{
    \[
        {
            "id":"49",
            "username":"failtime",
            "acct":"failtime",
            "display\_name":"failtime",
            "locked":false,
            "bot":false,
            "discoverable":false,
            "group":false,
            "created\_at":"2017-04-24T15:48:10.334Z",
            "note":"\\u003cp\\u003eProfessional loser.\\u003c/p\\u003e",
            "url":"https://mastodon.sdf.org/@failtime",
            "avatar":"https://mastodon.sdf.org/system/accounts/avatars/000/000/049/original/3fe810f812b83712.jpg?1567388103",
            "avatar\_static":"https://mastodon.sdf.org/system/accounts/avatars/000/000/049/original/3fe810f812b83712.jpg?1567388103",
            "header":"https://mastodon.sdf.org/system/accounts/headers/000/000/049/original/45632b66add48db3.jpg?1592224165",
            "header\_static":"https://mastodon.sdf.org/system/accounts/headers/000/000/049/original/45632b66add48db3.jpg?1592224165",
            "followers\_count":99,
            "following\_count":69,
            "statuses\_count":1880,
            "last\_status\_at":"2020-06-27",
            "emojis":\[\],
            "fields":\[
                {
                    "name":"Blog",
                    "value":"\\u003ca href=\\"https://failti.me\\" rel=\\"me nofollow noopener noreferrer\\" target=\\"\_blank\\"\\u003e\\u003cspan class=\\"invisible\\"\\u003ehttps://\\u003c/span\\u003e\\u003cspan class=\\"\\"\\u003efailti.me\\u003c/span\\u003e\\u003cspan class=\\"invisible\\"\\u003e\\u003c/span\\u003e\\u003c/a\\u003e",
                    "verified\_at":"2020-06-24T00:27:59.294+00:00"
                },
                {
                    "name":"Laifu",
                    "value":"\\u003ca href=\\"https://laifu.moe\\" rel=\\"me nofollow noopener noreferrer\\" target=\\"\_blank\\"\\u003e\\u003cspan class=\\"invisible\\"\\u003ehttps://\\u003c/span\\u003e\\u003cspan class=\\"\\"\\u003elaifu.moe\\u003c/span\\u003e\\u003cspan class=\\"invisible\\"\\u003e\\u003c/span\\u003e\\u003c/a\\u003e",
                    "verified\_at":"2020-06-15T00:37:19.734+00:00"
                }
            \]
        }
    \]
}

I _really_ don't want to have to go through that to create a struct out of it. I started digging around to see if there was a better way and, as is usually the case when dealing with code, came across a [Stack Overflow post on the topic](https://stackoverflow.com/questions/43624404/golang-how-to-parse-unmarshal-decode-a-json-array-api-response). Along with some other helpful information that I used to improve my code a little, one of the replies linked to [JSON-to-Go](https://mholt.github.io/json-to-go/). The service allows me to paste in JSON output like what I included above in this post, and it will automatically generate the corresponding struct. I tried it out, and it nicely gave me the following:

type AutoGenerated struct {
    ID             string        \`json:"id"\`
    Username       string        \`json:"username"\`
    Acct           string        \`json:"acct"\`
    DisplayName    string        \`json:"display\_name"\`
    Locked         bool          \`json:"locked"\`
    Bot            bool          \`json:"bot"\`
    Discoverable   bool          \`json:"discoverable"\`
    Group          bool          \`json:"group"\`
    CreatedAt      time.Time     \`json:"created\_at"\`
    Note           string        \`json:"note"\`
    URL            string        \`json:"url"\`
    Avatar         string        \`json:"avatar"\`
    AvatarStatic   string        \`json:"avatar\_static"\`
    Header         string        \`json:"header"\`
    HeaderStatic   string        \`json:"header\_static"\`
    FollowersCount int           \`json:"followers\_count"\`
    FollowingCount int           \`json:"following\_count"\`
    StatusesCount  int           \`json:"statuses\_count"\`
    LastStatusAt   string        \`json:"last\_status\_at"\`
    Emojis         \[\]interface{} \`json:"emojis"\`
    Fields         \[\]struct {
        Name       string    \`json:"name"\`
        Value      string    \`json:"value"\`
        VerifiedAt time.Time \`json:"verified\_at"\`
    } \`json:"fields"\`
}

I pasted it into my code, changed the name from `AutoGenerated` to something a little more fitting, and sure enough I was now able to `Unmarshal` my API response into a usable struct... without having to go through the pain of creating the struct myself. Huge kudos to the creator for such an awesome and useful service.
