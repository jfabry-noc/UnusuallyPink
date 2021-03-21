+++
title = "Making A Hash For The HTML Integrity Property"
date = "2020-07-21"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["css", "hash", "html", "integrity", "sha256"]
tags = ["css", "hash", "html", "integrity", "sha256"]
+++

I caused a little bit of chaos for myself the other night when I updated one of my websites. The site is a static, single page that I use for work-related bookmarks; it's basically a site I stood up to have _something_ at a domain I wanted to buy. While making a couple of changes to the links, I decided to update the background image. That was a little bit gross to do since the site was originally compiled with [Hugo](https://gohugo.io/), but after the initial setup I just modified the HTML directly. In this case I used Vim to search the minified CSS to see where the background image was specified and update it to the file I wanted.

After making that change, I refreshed the page to be greeted by this:

![](/images/MakingAHashForTheHTMLIntegrityProperty_html-e1599494214431.png)

Of course, originally I didn’t think to open the developer console. Instead, I just noticed that after making the change, _none_ of my CSS was loading. Cool. Checking the source HTML file, I quickly noticed that the `link` tag in the header where I tell it which CSS file to use had an `integrity` property. Following that property was a straight that started with:

```
sha256-
```

It was already pretty apparent what was happening. The `integrity` property is giving a SHA256 hash of my CSS file. Since I changed that file, the current hash no longer matched the specified hash. As a result, the CSS file was ignored. I verified this by removing the `integrity` tag and refreshing the page. Sure enough, everything now loaded as expected. Not wanting to leave it at that, though, I dug a bit more into the property. I eventually found a great [MDN article](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity) on it. The idea is that you specify this property for files you're pulling from a CDN, such as you'd commonly use for [Bootstrap](https://getbootstrap.com/docs/3.3/getting-started/#download-cdn). Since you don't control the source for those files, you can opt to not use them if they change without your knowledge. Pretty cool! The developer of my particular Hugo theme decided to include this in their code. While removing it fixed the issue, I figured it would be a good learning experience to use it, even if it didn't _really_ make sense considering I was hosting the CSS locally on the same server as the website.

The key was to figure out how to create the SHA256 hash. Without reading things completely (my bad; always RTFM), I first just created a hash the way I typically would:

```shell
shasum -a 256 ./my.css
```

I used the output of this to complete my `integrity` specification in HTML, and was greeted with the same disgusting, CSS-less webpage from the screenshot above. After a little more reading, I realized [the hash needed to be a base64 encoding of binary](https://stackoverflow.com/questions/54065781/how-to-generate-sha-for-integrity-html-tag). I tried again with the following commands:

```shell
openssl dgst -sha256 -binary ./my.css | openssl base64
```

Note that if you remove the pipe and the `-binary` switch, you'll see the ouput of the `openssl` command matches the output of the `shasum` command.

I tried replacing the old hash in my HTML file with the new one, refreshed the page yet again, and sure enough this time everything worked as expected. As I mentioned, it doesn't do me a ton of good in this scenario since the HTML and CSS files are all hosted off of the same machine that I control, so if someone else is changing that CSS file I have way bigger problems. It was a good learning experience for the future, though.

Stay pink!
