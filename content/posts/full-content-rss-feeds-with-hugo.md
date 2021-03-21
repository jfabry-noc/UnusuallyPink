+++
title = "Full Content RSS Feeds With Hugo"
date = "2020-08-23"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["hugo", "rss"]
tags = ["hugo", "rss"]
+++

Last week I made a test post on Mastodon linking to one of my blogs via `curl`. I was doing this to see how to best handle the formatting for a script I was working on to periodically check my blog and link to any new posts from Mastodon. My friend [tomasino](https://tilde.zone/@tomasino) who I’ve known for quite a while through SDF reached out to ask if there was an RSS feed for it. (Note that I now link to the RSS feed from the menu!) After he subscribed, he noticed that the RSS feed is only showing part of each post. It turns out that [my theme](https://themes.gohugo.io/hugo-theme-terminal/), like many others, is using the [default RSS template](https://gohugo.io/templates/rss/) in Hugo. This only publishes a portion of each post to the RSS feed, the idea undoubtedly being that people will navigate to the site to finish reading the content, allowing whatever trackers are in place to see this. Since I don’t have any trackers on my site and don’t care about hits, this just serves to be a pain in the butt for anyone trying to use RSS; I personally hate having to move out of my own RSS reader in order to finish reading something.

With a _lot_ of help from tomasino (who clearly knows a **significantly** more about RSS than I do), I started trying to modify my RSS template to include the full content for each post. Since the Terminal theme is using the default template, I started by taking the [base template content](https://github.com/gohugoio/hugo/blob/master/tpl/tplimpl/embedded/templates/_default/rss.xml) and placing that in `/layouts/_defaults/` as `index.xml` so that I had something to modify. The first thing that I did was modify the `<description>` tag so that it contained `.Content` instead of `.Summary`. This _did_ cause the full content to be displayed in the RSS feed but caused all sorts of HTML encoding problems. Next I tried modifying the `<description>` block again so that instead of:

```
.Content | html
```

It was:

```
.Content | safeHTML
```

This was… slightly better. It fixed the HTML encoding problems, but it also caused the paragraph tags to disappear, meaning each post was a wall of text. tomasino’s thought was that I needed a `CDATA` block, which I also saw [mentioned in the Hugo support forum](https://discourse.gohugo.io/t/full-text-rss-feed/8368/2). The problem I quickly ran into was that the block, which I was now adding in addition to the `<description>` block, needed to look something like this:

```
<content:encoded><![CDATA[{{ .Content | safeHTML }}]]></content:encoded>
```

Adding that directly to to the layout file cause the leading `<` get HTML encoded, thus breaking the entire thing. Back to hunting on DuckDuckGo, I found [several people](https://github.com/gohugoio/hugo/issues/1740) with the [same issue](https://github.com/gohugoio/hugo/issues/4242). While a few people in those threads had offered some solutions for how to properly escape things, tomasino ultimately found the [cleanest solution](https://github.com/ttys3/hugo-theme-terminal/commit/86c8d80e396b82f28927b659e0e0edd9d8959afe). After recompiling my site yet again, the encoding looked good, but the XML was still missing some metadata. Trying to open it in Firefox would give the following error:

> XML Parsing Error: prefix not bound to a namespace

It’s worth noting, since I was missing this initially, that Firefox will _not_ render the XML when you’re using the `view-source:` view. This makes complete sense, but I had overlooked it. You need to actually navigate to the file normally, e.g. https://my.site/index.xml. What was going on here was that I needed to define the namespace, which I did by just copying the same line from tomasino’s own XML file for a site of his:

```
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom" xmlns:content="http://purl.org/rss/1.0/modules/content/">
```

After this addition and yet another recompile of the site, everything finally started to appear correctly in RSS readers. Suffice to say I would’ve preferred if I could simply toggle something in my `config.toml` file to switch my RSS feed from a summary to the full text, but at least it’s possible to modify this on your own if you have to.
