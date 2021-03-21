+++
title = "It's Always DNS"
date = "2020-08-30"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["cradlepoint", "dns", "router"]
tags = ["cradlepoint", "dns", "router"]
+++

There's a saying among system administrators:

> It's always DNS.

Meaning that whenever there’s an issue, DNS is likely the culprit. This morning that adage proved itself yet again.

My home network is currently running off of a [Cradlepoint](https://cradlepoint.com/) router. Cradlepoint’s specialty is making routers that can leverage LTE, so my router is configured to use my home ISP as the primary WAN link, but it will fail over to a cellular connection if my home ISP is unavailable. This is pretty handy, especially considering that I now work from home full-time. That being said, mobile data isn’t cheap here, and the data plan the Cradlepoint is using is paid for by my company. While it’s nice to fail over to LTE while I’m trying to work, I _don’t_ want to be eating through LTE data while I’m just sitting on the couch watching Hulu. As a result, I’ve configured alerting from the router’s cloud management platform to notify me when a failover occurs so that I can troubleshoot the network and tailor my online activity accordingly if I’m going to be on LTE for a while.

This morning was basically one of the worst starts to a weekend morning where I want to hang out with a cup of coffee and catch up on my RSS feeds. I woke up to an email alert from a few hours prior letting me know that my router had failed over to LTE. It happened once around 6 AM for a few minutes, failed back over to my ISP network, and then maintained that for roughly 40 minutes before failing over to LTE again a little before 7 AM. The first step, which I could easily do from bed with my phone, was to check for any outages from my ISP. Logging into my account there showed me that there weren’t any known outages, though.

Finally being forced to shuffle out of bed and into the living room to get eyes on the situation, I saw that the lights on the modem looked normal. I logged into the router’s management interface and verified that everything looked correct. I rebooted the modem to be safe, and the Cradlepoint immediately reconnected to LTE rather than using my modem’s connection. I bounced the Cradlepoint, and the connection status persisted. I disabled LTE on the router, and it listed the Ethernet port as the current WAN link, which seemed good. I tried connecting to [Laifu.moe](https://laifu.moe), though, and it wouldn’t load up. I tried to ping one of the [OpenDNS](https://www.opendns.com/) servers of `208.67.220.220` and also got no response. This was a critical mistake, though I didn’t know it yet at the time.

Thinking now that maybe something was up with my Cradlepoint, I pulled a bin of miscellaneous tech stuff out of the closet and fished through it to find the router from my ISP that I never use. I plugged that in line after my modem, removing the Cradlepoint from the equation, and bounced the modem. The ISP-provided router came online right away with the characteristic blue light that indicates everything is fine. I connected my laptop to its WiFi network and tried to load a webpage… with no success. I once again tried to ping `208.67.220.220` also without any response.

This was when I finally realized the flaw in my troubleshooting. Both the Cradlepoint and my ISP-provided router had been configured by me to use the OpenDNS servers as what they hand out with DHCP leases. Literally all of my devices are using `208.67.220.220` and `208.67.222.222` as their DNS servers. Likewise, the Cradlepoint needs something it can test to determine if a WAN link is up or down so that it can fail over to LTE and fail back to the Ethernet WAN link. I had _that_ set as `208.67.220.220` as well. So what if **that** was the problem? While still connected to my ISP router’s WiFi network, I tried to ping `8.8.8.8` and immediately got a response. OpenDNS is what was unreachable.

Ripping the ISP router out of the network, I linked the Cradlepoint back up. I reconfigured it to use [1.1.1.1](http://1.1.1.1/) as the DNS servers it hands out, and to leverage that for the state of the WAN link. As soon as I did that, everything began working and the Cradlepoint failed back to the Ethernet WAN link on the next check. I should probably rethink this setup where I’m using the same IP address for DNS as I am for the state of the WAN, but I should also remember that it’s always DNS and check that a little earlier in the process.
