+++
title = "Chromebooks, VPNs, and You"
date = "2019-09-13"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["chrome", "chrome-os", "google", "linux", "pia", "private-internet-access", "tunnelbear", "vpn"]
tags = ["chrome", "chrome-os", "google", "linux", "pia", "private-internet-access", "tunnelbear", "vpn"]
+++

Do you trust your Internet service provider? If you answer that question with a resounding “No!”, then you’re among friends here. While Internet connectivity is an essential need for many of us, there are a plethora of reasons to be wary of the company providing that connectivity to you. Especially in the United States, the [recent repeal of net neutrality rules](https://en.wikipedia.org/wiki/Net_neutrality_in_the_United_States#Rollback_of_Obama-era_rules) and [erosion of privacy](https://www.eff.org/deeplinks/2017/04/heres-how-protect-your-privacy-your-internet-service-provider) mean you may want to obscure exactly what you’re doing from your Internet provider, even if that activity is completely legal and innocuous. Just because I’m not doing anything illegal online doesn’t mean I want to allow my ISP to gather data on what I’m doing in order to sell it to advertisers; in my opinion they make plenty of money off of the monthly fee I pay them for Internet access.

This is where a VPN (virtual private network) can come in handy, as we discussed in [Episode 8](https://www.unusually.pink/podcast/episode-8-playlists-and-privacy). VPNs are historically most common in the corporate world. They allow employees to create a secure tunnel between wherever they are and their internal company network in order to access resources that aren’t available to the outside world. In the consumer space, though, they’ve been gaining popularity as privacy-conscious consumers look for way to protect themselves from things like public WiFi and, increasingly, ISP data collection.

_Note: This post is_ **_not_** _going to cover the potential risks for a VPN or how to choose one. We recommend you check out guidance from sources like the_ [_EFF_](https://ssd.eff.org/en/module/choosing-vpn-thats-right-you) _for that. Brandi and John personally recommend either_ [_Private Internet Access_](https://www.privateinternetaccess.com) _or_ [_TunnelBear_](https://www.tunnelbear.com)_. Check out_ [_Episode 8_](https://www.unusually.pink/podcast/episode-8-playlists-and-privacy) _for more details!_

As we’ve discussed in a few podcast episodes, I happen to be a fan of Chromebooks as my personal laptop. While I have a beefy desktop to use as a gaming computer or when I need to do some heavy lifting, I like having a Chromebook as a cheap, fast, and easy computer for things like browsing Reddit, watching YouTube videos, and typing up blog posts. The [addition of a Linux VM](https://www.unusually.pink/blog/unusually-pink-impressions-acer-chromebook-315) means I can even do some programming. VPNs get a little wonky on a Chromebook, though. Most VPNs offer applications for Windows, macOS, Android, and iOS. Many services will also support configurations with the [OpenVPN Connect](https://openvpn.net) client in case you want to use something open source and/or are running some flavor of Linux. For example, Brandi subscribes to [Private Internet Access](https://www.privateinternetaccess.com). On her MacBook, she simply runs the PIA application, selects the endpoint she’d like to direct her traffic to, and is done.

The waters get murkier in Chromebook land since you don’t install traditional applications on them. You also have to consider the scope of where you’re working on a Chromebook and what it is you’re looking to protect. Are you just concerned about securing the data flowing through the Chrome browser? Do you need to cover system level networking? Are you doing anything online via your Linux VM that you want to secure? These all play a role, and hopefully this post will enlighten you as to the reach of the options at your disposal.

The first step to testing the scope of VPN clients on Chromebooks is to be able to figure out what your external IP is since that can tell you where the outside world sees your connection coming from, be it your home network or a VPN provider’s. I personally like [I Can Haz IP](http://icanhazip.com/) for that. Simply going to the site will give you a web page with your public IP address. Here we can see the result I get from my Chromebook with no VPN solutions at play.

![](/images/ChromebooksVPNsandYou_browser_none.png)

The really cool part about this service is that if you bounce an HTTP request off of it from [curl](https://curl.haxx.se), it’ll reply with your public IP address. This easily allows testing of VPN providers from a command line. If you don’t have curl in your Linux VM, you can easily install it via:

```shell
sudo apt install curl
```

Here we can see what happens when I curl against I Can Haz IP from my Linux VM on my Chromebook. Note that it matches the public IP I get from my browser.

![](/images/ChromebooksVPNsandYou_terminal_none.png)

If you have an older Chromebook that doesn’t feature Google Play support, there are several VPN providers (including both [PIA](https://chrome.google.com/webstore/detail/private-internet-access/jplnlifepflhkbkgonidnobkakhmpnmh) and [TunnelBear](https://chrome.google.com/webstore/detail/tunnelbear-vpn/omdakjcmkglenbhjadbccaookpfjihpa)) that offer Chrome extensions. These can be great for quickly proxying your traffic in a pinch. After flipping the switch on the TunnelBear extension, for example, I can see that the endpoint I’m seen as coming from via my browser has changed.

![](/images/ChromebooksVPNsandYou_browser_extension.png)

It’s important to keep in mind, though, that the VPN is operating at the _application_ level rather than at the system level. Only traffic from my browser is going through the VPN. As a result, running curl again has no change; my Linux VM’s traffic is still going straight out through my ISP.

![](/images/ChromebooksVPNsandYou_terminal_extension.png)

This is what we refer to as a bummer. If you happen to have a Chromebook with Google Play support, though, there’s a better solution available. [Updates to Chrome OS 75 in the spring of this year](https://www.aboutchromebooks.com/news/chrome-os-75-android-vpn-support-linux-project-crostini/) resulted in better integration between Android VPNs and Chrome OS as a whole. Installing an Android VPN client from the Play Store and connecting it will result in the WiFi icon in Chrome OS changing to display a tiny key icon, just like you’d see in the notification area of Android. After making this connection, I can verify that my browser shows my connection as coming from my VPN provider.

![](/images/ChromebooksVPNsandYou_browser_android.png)

Even better, though, checking from my Linux VM now shows the same thing; the VM’s traffic is now also going through my VPN provider instead of to the prying eyes of my ISP.

![](/images/ChromebooksVPNsandYou_terminal_android.png)

Suffice to say, this is _much_ better. While the Chrome extensions are passable for older Chromebooks without Google Play access, the corresponding Android applications will offer far superior coverage if they’re an option on your particular device. Not that you shouldn’t have already been doing this anyway, but this should be an incentive to avoid purchasing the _insanely_ cheap Chromebooks that so frequently go on sale; I’d recommend making sure you get a device that at least has Google Play and Linux support. Keep encrypting that traffic, and stay pink!

_Note: In my testing, the Linux VM in Chrome OS would often struggle to reconnect properly after an Android VPN application was connected and/or disconnected. For the best results, I’d recommend launching the VM_ **_after_** _connecting your VPN. If you forget and connect your VPN after the fact, shut down your VM and restart it._
