+++
title = "Switching To Squarespace"
date = "2019-04-20"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["squarespace", "web-development", "website"]
tags = ["squarespace", "web-development", "website"]
+++

If you happened to see this site around the time when the [first post](https://www.unusually.pink/blog/its-all-about-being-unusually-pink) went up, you might notice that:

1. The site looks very different now.
    
2. A lot of what’s in that first post no longer seems to be true.
    

For example, this is _not_ the [Rusty](https://themes.gohugo.io/hugo-theme-rusty/) theme for Hugo. Those technically savvy would also notice that the site no longer has the same DNS record value as [laifu.moe](https://laifu.moe) where things were originally hosted. That’s because the site is no longer running on my own web server and is not created using Hugo. There were a few reasons for this. The main was just that I’m **really** bad at web design. The Rusty theme in Hugo is pretty light on imagery, which I’m cool with. Once we decided to actually make _Unusually Pink_ into a thing and do a podcast, though, we had our **amazing** logos made by the uber-talented [JPFDesigns](https://www.jpfdesigns.com/). Integrating those into the Rusty theme for Hugo was a bit more than I was up for; CSS is legitimately the final boss of my life, and my life is (apparently) an NES _Contra_ game; I couldn’t do it.

The other reason was just that it allows for **much** better reliability. The site isn’t beholden to my ability to not mess up my web server. Not that it’s particularly likely for me to do something to brick it (I’ve been using Linux and Nginx for my web servers for ages now), but it’s possible. I was also responsible for backups, which I’d prefer to take off of my own hands if possible.

The last reason was that the site really needed to be divided from one main section (e.g. the blog I originally planned just to do **something** with the domain) to two sections: a blog and a list of podcast episodes. While I was able to just dump a **/podcast** directory into my **static** folder for Hugo, it meant that posting podcast episodes and summaries was now an entirely manual process rather than something assisted by a CMS-esque system like Hugo.

Swapping to [Squarespace](https://www.squarespace.com/) allowed me to let someone far smarter than me figure out all of that within a theme; all I had to do was upload some images (Squarespace is awesome at scaling images for me, even when it needed to make one tiny for the favicon) and then swap around a few of the colors in the theme to get something _unusually pink_. I was also able to simply add two blogs to the site; one is a normal blog and the other will have posts for each podcast episode. In this way, both sections of the site are managed by a CMS rather than being done manually. Doing it manually may not seem like **too** big of a deal at first, but once you start to get too many posts for a single page, creating and manually updating the pagination after each new post would be enough to drive someone insane.

As for choosing Squarespace, it’s the one I’ve heard the most about through various avenues on the Internet. Their pricing was reasonable, and I figured it seemed like a safe bet since I know a few other people who have experience with them. The other recommendation I got was Wix, which I admittedly had never heard of previously. Looking at the [pricing for Wix](https://www.wix.com/upgrade/website) compared to the [pricing for Squarespace](https://www.squarespace.com/pricing), though, I think it’s clear that Squarespace is a better deal. The Wix $11 USD per month package is pretty lackluster, especially when you look at 2 GB of bandwidth and 3 GB of storage. To get something more comparable to Squarespace’s $12 per month package that includes unlimited bandwidth and storage, you’d need the $14 per month plan from Wix… and that still doesn’t give you unlimited storage.

Expect the site to still go through a few minor changes as we continue to tweak the layout, colors, and everything else. Feel free to drop any feedback to our [Twitter profile](https://twitter.com/UnusuallyPink)!
