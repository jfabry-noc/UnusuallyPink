+++
title = "Twenty Twenty-One WordPress Theme"
date = "2021-01-11"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["dracula", "dracula-theme", "twenty-twenty-one", "web-development", "website", "wordpress"]
tags = ["dracula", "dracula-theme", "twenty-twenty-one", "web-development", "website", "wordpress"]
+++

While I made the change a few weeks ago, it seems fitting that my first post of 2021 should be about my swap to the Twenty Twenty-One WordPress theme. For anyone who isn't aware, each year WordPress releases a new "official" theme for the platform. While theme-elitists out there may scoff at them in favor of paid, premium themes, I've always found the yearly offerings to be elegant and well-designed. I had _originally_ planned on putting this post together on January 1st, but what can I say... movies on HBO Now aren't going to binge-watch themselves.

While I've typically enjoyed the yearly WordPress theme releases, the previous iteration of this blog was actually back on the Twenty Sixteen theme since the yearly releases after that one didn't feel like they were a particularly good fit for a blog. I still liked the look of the Twenty Sixteen theme, especially after I switched the colors around to match the [Dracula Theme](https://unusually.pink/all-dracula-everything/), but the fact remains that 2016 is more or less ancient history in the world of web design. As a result, parts of the site just felt a bit... antiquated. For example, while the banner at the top allowed me to show off the Unusually Pink graphics that we originally commissioned for the podcast, it left the site feeling just a bit old.

![](/images/cropped-cropped-Unusually-Pink-Facebook-Cover-Photo.png)

The artwork is still amazing, though.

While I spent a decent bit of time tweaking the Twenty Sixteen theme to suit my needs, once I saw the [Twenty Twenty-One theme announcement in September I decided to stop further tweaking and try being patient](https://make.wordpress.org/core/2020/09/23/introducing-twenty-twenty-one/). I also followed a bit [of the progress on the GitHub repository](https://github.com/wordpress/twentytwentyone), but not being a web developer meant that it didn't really give me a good vibe for how the theme would feel in practice.

Once the theme released, though, I immediately found myself drawn to how minimalist and streamlined it is. I feel like it provides me with the bare necessities, and then I can expand upon it from there to fit my needs without a lot of unnecessary overhead. One look at the current layout at the time of this writing probably clues you in to the fact that these aren't the default colors; I once again opted to update the design to feel like the Dracula theme. While Twenty Twenty-One offers less in the customization UI for modifying the colors, it can still easily be done via CSS (a level of access that I sorely missed when [the site was hosted on Squarespace](https://unusually.pink/unusually-pink-migration/).) I'm not sure if there's a better way to do it than just opening the published site in my browser's developer tools and using the inspection feature to get the classes of the objects I want to change, but that ended up getting me to the end result I wanted.

Along with the colors, I also felt the need to adjust some of the font sizes. In the default setup, Twenty Twenty-One opts for **big** fonts, and while they looked kind of nice on my brand new MacBook Pro with a Retina display, on my 1080p, 11" [Star Lite Mk III](https://unusually.pink/star-lite-mk-iii/) going to a post would result in just the header and the post title being visible. None of the actual content could be read without scrolling. I wasn't thrilled about my site looking so gigantic on lower resolution displays, so many of the font sizes were tweaked. That being said, as someone with extremely poor vision I'm a fan of large-ish fonts, and I didn't want to completely flip to tiny text. Readability is, arguably, one of the most import things on a text-heavy site.

If anyone is curious, my full CSS customizations are below. Hopefully it's not too offensive since frontend is most definitely not my specialty.

<script src="https://gist.github.com/jfabry-noc/61638fc4518fd5d09df4b014a38543fd.js"></script>

While there may still be some minor adjustments for sizes and spacing, for the most part I like where things are at. I've historically disliked blog landing pages which only showed the beginning of each post, but I've done a bit of a 180 in that regard. With Twenty Twenty-One I've been appreciating how the front page is much less cumbersome, especially when trying to reach the links or search box at the very bottom of the page. I think it also provides a bit of a cleaner focus on the [link for the About page](https://unusually.pink/about/) that I link to on most of my online profiles.
