+++
title = "RIP Chrome OS"
date = "2020-02-02"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["chrome", "chrome-os", "chromium", "google", "tab-discarding"]
tags = ["chrome", "chrome-os", "chromium", "google", "tab-discarding"]
+++

If you’ve listened to some of our episodes or [read some of my other posts on this site](https://www.unusually.pink/blog/unusually-pink-impressions-acer-chromebook-315), then you’re likely aware that I’ve historically been a Chromebook fan. I started with the original HP Chromebook 11 and have owned quite a Chromebook collection since then. The speed and simplicity to do the two main things I need in my personal life (interacting with web pages and making SSH connections) coupled with the low cost made them a perfect match.

My feelings about Chrome have been shifting, though, as I [elucidated in my post about switching back to Firefox as my main browser](https://www.unusually.pink/blog/back-to-firefox) on my work laptop. That post was written from the context of Chrome as a browser without really looking specifically at the Chrome OS operating system. In that area Google has also been making some… less than stellar changes which have ultimately caused frustration to the point where I’ve now given up on Chromebooks entirely. The primary culprit: Google’s confusing war on swap.

_Note: Really smart people make Chrome and Chrome OS. I don’t mean to throw shade at them. I’m sure there’s well-reasoned thought behind the changes I’m about to describe that is either not posted or not posted in a location I found. The real-world impact is undeniable, though._

## The Symptoms

Before diving into the problem, I’d like to discuss the end user experience. I admittedly hadn’t been following the Chrome change logs over the past few months while I’ve been busy with starting a new job and moving; I’d mark those posts as read in Feedly and move on to more interesting things. So it seemed almost out of nowhere that my tabs and PWAs in Chrome OS would simply die and reload on their own. Sometimes it wasn’t a big deal. A tab with a news article or a Reddit post can reload without any dire consequences. In other cases, it was a bit more annoying: the Spotify PWA would reload, causing the music I was enjoying to stop playing. And in some instances it was catastrophic: I’d tab away from Squarespace where I had been working on a post for this site to verify some information, and the tab would reload when I went back to it, causing me to lose my progress. The same thing would happen with the [Secure Shell extension](https://chrome.google.com/webstore/detail/secure-shell-app/pnhechapfaindjhompbnflcldabbghjo?hl=en), kicking me out of my SSH session and causing me to lose everything if I didn’t have tmux running. Even if I **did** have tmux running, it’s still insanely annoying to have to log back in every time I flip from an ebook back to my SSH session… assuming the ebook didn’t **also** reload, causing me to have to find my place again.

As you can likely imagine, this was maddening and quickly made it impossible for me to really _do_ anything with my Chromebooks. At this point, it seemed like my device was in open rebellion against me, and I started doing research into what was happening under the hood to cause this behavior.

## The Culprit

The cause of this behavior ended up being a new “feature” referred to as [Tab Discarding](https://developers.google.com/web/updates/2015/09/tab-discarding?authuser=0). The idea seems fine on the surface; not all open tabs in a browser are **active** tabs. So to avoid low memory conditions, Google was discarding tabs that it deemed inactive. This drops the tab and its contents out of memory but keeps the tab open in the browser. If you open that tab again, the contents are then reloaded. The benefit tot his is that you can keep more memory available for actively utilized tabs without having to drop down to swap.

### A Note On Swap

If you aren’t familiar with swap, it’s good to think about what computers do when they run out of memory. Since just crashing isn’t exactly an option, for the past few **decades** computers have utilized what’s referred to as swap space. In most flavors of Linux, swap is an actual partition on the disk. In Windows you’ll have a **pagefile.sys** file. The point of both is the same; when the operating system runs out of memory, it’ll take some data currently stored in memory that hasn’t been used in a little while and write that to the disk in either the swap partition or the pagefile. Then that memory becomes available for some new, active process to utilize. Should the content that was swapped be required again, something _else_ will be transferred from memory to disk, and then the previously swapped content can be moved from the disk back to memory. Most systems will give you insight into how much swap you’re using. For example, we can see from running **htop** on this Linux laptop that I’m not currently leveraging swap:

![](/images/RIPChromeOS_htop_swap.png)

This matters because reading and writing from/to the disk is slower than just working in memory… though it’s not nearly as slow in the world of SSDs than it was in the world of spinning platter hard drives. The less swap you can use, the snappier things go… though I question if waiting for SSD swap is actually slower than waiting a few seconds for a web page to reload. Regardless, not using swap can be a matter of both adding more physical memory to the system and optimizing the software for it. Okay, back to the main point.

### Back To The Culprit

Google was attempting to use software optimizations to make Chrome stop leveraging swap and keep everything in active memory. The problem is that while this is good in theory, it doesn’t pan out so well in practice in a world of Chromebooks that have 2 - 4 GB of memory installed. I was hitting the point on my Acer Chromebook 315 (which has 4 GB of RAM) where two tabs were too much for the system. Flipping between the two would cause the tab I just left to be discarded. I tried to even go so far as to disable the Play Store and Linux support to see if that would help free up additional memory. While it helped a _little_, in the end I was still experiencing the same problems. My laptop was a paperweight.

### Solutions?

I naturally started looking for solutions to this problem. The Chromium project put together a [decent FAQ on the behavior](https://www.chromium.org/chromium-os/chromiumos-design-docs/tab-discarding-and-reloading). While that confirmed for me _why_ it was happening, it didn’t exactly provide solutions on how to make it stop. For the question of “How can I make it stop?” the answer was:

> Close some tabs or uninstall extensions that take a lot of memory. If there's a specific tab you don't want discarded, right-click on the tab and pin it.

Except the order of operations for discarding tabs goes:

1. Internal pages like new tab page, bookmarks, etc.
    
2. Tabs selected a long time ago
    
3. Tabs selected recently
    
4. Tabs playing audio
    
5. Apps running in a window
    
6. Pinned tabs
    
7. The selected tab
    

So even pinning a tab won’t necessarily prevent it from being discarded; it’s just less likely. It also seems legitimately **insane** to me to have to temporarily pin any tab you’re actively working in to keep it from being discarded. The behavior is clearly detrimental and needs to be disabled. The original post for the “experiment” mentioned being able to toggle this feature from:

chrome://flags/#enable-tab-discarding

Only that didn’t actually work for me; the option was gone. I noticed that [the page was last updated in January of 2019](https://developers.google.com/web/updates/2015/09/tab-discarding?authuser=0), so I’m sure this behavior had changed. The post had also mentioned going to the flags page in order to _enable_ tab discarding when it was clearly now on by default.

Going further down the rabbit hole, i found some [threads on Reddit](https://www.reddit.com/r/chrome/comments/bxckh3/no_more_automatic_tab_discarding/) where users confirmed my fear; starting with Chrome OS 75 this “feature” was forcibly turned on and the flags to disable it removed. Yikes! The only solution present was to install [some random browser extension to prevent tab discarding](https://chrome.google.com/webstore/detail/disable-automatic-tab-dis/dnhngfnfolbmhgealdpolmhimnoliiok?authuser=0). I can only speculate as to why the ability to turn off such anti-user behavior was removed, but having to install an extension to get it back seems absurd, especially when, as the author of the extension notes, it still isn’t guaranteed to prevent your tabs from being discarded.

At this point I decided that Chrome OS had finally crossed the line of what I was willing to tolerate in the name of simplicity. When I literally can’t use my laptop to author a post for this website, the device is valueless to me. I won’t go into what I’ve opted to replace my Chromebook with in this post (that’s a topic for another post or possibly even a podcast episode), but suffice to say I’ve officially hopped off the Chrome OS train. The good news is that I’m now not using the Chrome browser _anywhere_, giving Google one less avenue to capture data on what I’m doing online.
