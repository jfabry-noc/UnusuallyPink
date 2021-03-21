+++
title = "Microsoft Edge Insider: It's Actually Not That Bad"
date = "2019-04-25"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["browser", "chromium", "edge", "microsoft"]
tags = ["browser", "chromium", "edge", "microsoft"]
+++

If you happened to listen to our [Introduction Episode of the podcast](https://unusuallypinkpodcast.podbean.com/e/episode-0-introduction-1556126516/), you’ll know that Brandi and I are sysadmins who work together in a _highly_ Microsoft-centric environment. Essentially all of the servers we manage run some flavor of Windows, we make heavy use of Office 365 and Azure, and both of us spend our entire day either typing email into Outlook or commands into PowerShell.

You may read that and think, “Wow, they really like Microsoft stuff.” At least for me (John, if that’s not clear by now) that’s not the case. I tend to be extremely critical and frequently frustrated with a lot of Microsoft’s offerings. I’ve poured a healthy bit of salt into the Internet over the years at Microsoft’s expense. I could even share some of my more recent frustration with Microsoft products if it wouldn’t spoil what will likely be content for a near-future podcast episode. Suffice to say, there’s a reason one of my former coworkers threw together this image… that’s my face on the can if you’re confused.

![](/images/MicrosoftEdgeInsiderItsActuallyNotThatBad_morton.png)

And there’s a reason why I made this image at one point. Cut me some slack… I didn’t really know how to use [GIMP](https://www.gimp.org/) that well at the time.

![](/images/MicrosoftEdgeInsiderItsActuallyNotThatBad_admiral_o365.png)

That being said, I’m all about trying the new hotness where software is concerned, so I decided to install the [new Microsoft Edge Insider build](https://www.microsoftedgeinsider.com/en-us/) on my work machine. If you haven’t been keeping score at home, Microsoft essentially admitted that the current, production version of Edge that ships with Windows 10 is a disaster. No one uses it according to every metric, despite Microsoft’s attempts at forcing it on users. They also accepted that their rendering engine under-performed. Perhaps the biggest problem, though, was how the Edge browser was inextricably tied to the underlying operating system, meaning that Edge essentially only got updates during Microsoft’s major updates to Windows 10. Going 6+ months between browser updates is a pretty massive blow when competitors are releasing new versions every month.

The new builds of Edge are based on the open-source [Chromium](https://www.chromium.org/) project. While perhaps most notably serving as the underpinnings for Google’s behemoth of a browser that has come to dominate the web, [Chrome](https://www.google.com/chrome/), it’s also come to serve the same function for plenty of other noteworthy browsers, such as [Opera](https://www.opera.com/) and [Brave](https://brave.com/).

So I threw the Insider build on my work PC to check it out. If you happen to be curious, you can install it alongside the current version of Edge. The icon looks the same but has a green “Dev” stamp over the blue “e”. If you fire it up, it looks essentially like what you’d expect to see opening the current version of Chrome. You get the option to sign in with a Microsoft account, though, rather than a Google account. The various options and settings have been changed up in how they appear, but they’re basically all the same. If you’d like to use an extension, you can select from a subset of Chrome extensions that are currently working with this build of Edge in Microsoft’s own gallery; I’ve read that they’re working to make every extension supported by Chromium and Chrome work with Edge eventually. I was just happy enough that I could get [uBlock Origin](https://github.com/gorhill/uBlock).

![](/images/MicrosoftEdgeInsiderItsActuallyNotThatBad_edge_chromium.png)

I’ve been running with it as my default browser for a couple of weeks now, and I have to admit that I haven’t run into any issues yet. Everything just works the same as I’d expect, only with a bit of a smaller footprint than with Chrome. I assume this is [due to all of the Google services Microsoft ended up removing](https://www.theverge.com/2019/4/8/18300772/microsoft-google-services-removed-changed-chromium-edge-browser). If so, it almost reminds me of how Firefox was before the Electrolysis rebuild, where it had become a bit slow and clunky due to bloat. Or how Opera was back in the day when the browser featured a mail client, IRC client, toaster oven, vacuum cleaner, etc. A good feature purge isn’t necessarily the worst thing to happen to some software projects as they expand over time.

Speaking of [Firefox](https://www.mozilla.org/en-US/firefox/new/), one of the downsides to this is the fact that it means yet **another** browser is now based on Chromium. Outside of looking as extremely niche browsers, this means that Firefox is now the only non-Chromium-based mainstream browser. That’s a sticky situation since it means that a single project (Chromium) can essentially dictate the growth and direction of the web if they so choose. If that project opted to move away from established standards to do their own thing, for example, no one would be able to ignore that many impacted users. Switching to a Chromium base is a situation that I would describe as good for Microsoft and extremely bad for the web as a whole.

And if you’re one of the three people in the world using the current stable version of Edge with Windows 10 and you’re mad that it will be going away, please just read [this article about a nasty Edge vulnerability that Microsoft has declined to patch](https://www.forbes.com/sites/daveywinder/2019/04/21/unpatched-windows-10-vulnerability-uses-microsoft-edge-to-steal-data-how-to-mitigate-the-risk/#503de7ad673f). Then please stop using that browser.
