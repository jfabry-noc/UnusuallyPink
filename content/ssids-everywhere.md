+++
title = "SSIDs Everywhere"
date = "2020-10-25"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["google-wifi", "mesh-network", "wifi"]
tags = ["google-wifi", "mesh-network", "wifi"]
+++

As someone who has been an apartment dweller for a relatively long time, there are some extremely solid perks to it. It's nice to never have to worry about things like maintenance, for example; if something goes wrong, I open a service ticket and someone shows up to fix the problem. When my furnace wouldn't start on a cold day, for example, I wasn't scrambling to figure out who to call. There are some downsides as well, though. For one, many people are frustrated by the fact that they can't customize their home to the degree they'd like with respect to things like the color of the walls. Anyone who knows me knows that I couldn't possibly care less about that. What _does_ give me grief, though, is how significantly the spectrum around me is completely polluted by my neighbors.

![](/images/ssids.png)

I had issues with this in my last apartment, which was roughly the same size as my current apartment but with a longer, narrower layout than my current home. When I moved my home office to the bedroom, which was at the opposite side of the apartment from where my router was, I saw a noticeable performance decrease in my home network; this was a problem when I was working from home and an even **bigger** problem when I was trying to reach Platinum in Overwatch (spoiler alert, I didn't manage to do it.) At the time, I replaced my cheap home router with a mesh WiFi setup so that I could utterly drown out my neighbors with sheer WiFi dominance. I ended up buying a pack of 3 access points because it was only slightly more expensive than buying 2 access points individually. That was more than enough to blanket my 900 square foot apartment, and I didn't really think too much about my setup after that.

Fast forward to my current apartment, which is located in a much more populous area than where I previously lived. Pre-pandemic, I still didn't think too much about my home network setup. The only bandwidth-intensive activity I did was video streaming, and that was mainly done from streaming sticks connected to my TV that was literally right on top of the router/access point connected to my modem; I never ran into issues with it. After the pandemic kicked into high gear in this area, though, I started living from my home office and spending unholy amounts of time on web conferences. While they worked well enough most of the time, I'd periodically have spikes of extremely high latency that would cause me to sound like Megatron's cousin while on calls. This was annoying on work calls and infuriating when [trying to record podcast episodes](https://unusually.pink/the-dumpster-fire-of-remote-podcasting/).

At first I assumed that the problem had to be upstream with my ISP being overloaded since suddenly everyone was staying home all the time. As the problem persisted, though, it made less and less sense to me. It would be reasonable if this behavior happened during the evenings when everyone is sitting around binge-watching their favorite shows because they can't go out. When my network is choking to death on a 9 AM call, I was scratching my head. Surely not enough people could be doing video calls at that time, right? While latency affects VOIP traffic heavily, it isn't exactly the most bandwidth-intensive thing to be doing in 2020.

Thinking my mesh network was the problem, I even tried ripping it out and replacing it with a single router connected to my modem. While it at first seemed to have a bit more stability, I still ran into some of the same problems. Finally, I realized that I was seeing a _lot_ of networks when I just looked at what was available from any of my client devices. I fired up WiFi Explorer and was presented with this nightmare.

![](/images/wifi_explorer-1024x456.png)

If you're thinking that looks disgusting, you're correct. Every channel in both the 2.4 GHz and 5 GHz bands is completely packed with networks. I've been checking this periodically since realizing it could be the problem, and I regularly see anywhere between 50 and 70 different wireless networks from my apartment. Yikes.

Admittedly, I'm part of the problem. I'm broadcasting with a 5 GHz and a 2.4 GHz network from a router that I use exclusively for work. I'm also broadcasting a main and guest network on my main mesh setup. That causes matching SSIDs to broadcast on both the 2.4 GHz and 5 GHz spectrum on _each_ of the 3 access points _per network_, meaning I'm technically broadcasting 12 SSIDs. Even so, I'm still competing with, at minimum, 40-ish other devices crowding the same spectrum.

After coming to the realization that I may have been blaming all of the wrong things, I adjusted the setup of my access points so that the main router/access point which is connected to my modem is in direct line of sight across my apartment from the mesh access point at my office desk (which moved from the desk proper to a table next to the desk.) Since doing that, knock on wood, things have at least _seemed_ to be a bit more stable for me. Either I've been having a better experience on web conferences or no one bothers to complain to me about it when my audio suddenly sounds like garbage because they're just used to that happening from my end.

All that being said, I do still have an issue where the network stack on my MacBook Pro will crash, leaving me with no network connectivity until I disable and then re-enable WiFi. I haven't managed to find a fix for that particular problem yet, though I imagine having 4 different flavors of VPN client installed probably isn't doing me any favors.
