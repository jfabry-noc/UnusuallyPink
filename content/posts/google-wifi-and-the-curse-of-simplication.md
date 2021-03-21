+++
title = "Google Wifi And The Curse Of Simplication"
date = "2020-03-11"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["google-wifi", "ipad", "iphone", "mesh-network", "wifi"]
tags = ["google-wifi", "ipad", "iphone", "mesh-network", "wifi"]
+++

As you likely know if you listen to the podcast, about 6 months ago I [started a new job](https://www.unusually.pink/podcast/episode-16-abandonment-and-homebrew), and about 3 months ago I moved to be closer to that job. With Brandi's help, I managed to get moved without too much hassle, but I cut a few corners in getting my new home set up. One of those corners was my home network; after using the "kit" from my ISP to get my Internet service activated I had never bothered to swap out their equipment with my own. Given the number of devices I had connected to the Wifi (2 laptops, a phone, a tablet, 2 streaming sticks, 3 smart speakers, etc.) I didn't want to bother with getting _everything_ connected to a different network.

That being said, I've been spoiled by mesh Wifi and coverage in my bedroom was occasionally problematic; in apartments like mine it's easy for crowded channels to drown out your signal. I had a little free time one evening earlier this week and decided to finally bite the bullet.

My Wifi setup at my last apartment was [Google Wifi](https://store.google.com/product/google_wifi_first_gen) with 3 access points. 3 APs may seem overkill for a roughly 900 square foot apartment, but when I bought them the bundle of 3 was essentially the same price as buying 2 individually... plus it ensures that I have **total Wifi dominance** over my neighbors.

The initial, out-of-box experience with setting up Google Wifi was fairly simple. Connect the first AP to the modem, install the Google Wifi mobile app, have it discover the AP, and then configure it through the app. Once your Wifi is working, repeat the process for the additional APs to get them to form the mesh network. The full instructions are available [here](https://support.google.com/wifi/answer/7183148?hl=en).

However, configuring them when they've already been configured once turned out to be _much_ more irritating and far less straightforward. Given that I had a brand new phone, I needed to install the Google Wifi app. I did that and logged in with my Google account. Naturally, Google had stored the information about my APs and how they were configured, but told me they were offline. Taking one of the 3, I connected it to my modem and bounced the modem. The light ring on the AP stayed orange, indicating that it didn't have a WAN link.

I can only assume this is where I made the first of many mistakes that I'll take partial responsibility for and partially blame on how kludgey this whole experience is when you're tied to a mobile app. Rather than repeating this process potentially two more times to find the AP that was expecting to be the router (and that assuming the memory was still held despite being off for months), I figured I would just factory reset the APs and start from scratch. I could select the factory reset option from the app to clear the APs out of it, but naturally that wouldn't do anything to the physical devices since they couldn't connect to the Internet to see that configuration. A quick web search let me know that I needed to hold the button on the back of the AP for 10 seconds while the device is on, pull the power while continuing to hold the button, and then plug the power back in while **still** holding the button until I saw the light ring. Talk about a secret handshake. Regardless, I did that, and then proceeded to wait until the light ring began to pulse blue. That indicates it's ready for setup, per the [aforementioned instructions](https://support.google.com/wifi/answer/7183148?hl=en).

At this point, I open the app on my phone. It does a quick search, confirms for me that it **sees** the AP, and then starts trying to connect. For the next 15 minutes I stare at this:

![](/images/GoogleWifiAndTheCurseOfSimplication_PhotoMar092C54634PM.png)

Umm... not great. At this point, I figure something must be amiss in the app. The instructions in step #4 say I should be prompted to scan the QR code on the bottom of the device... a prompt that I never receive. I kill the app and re-launch. I get the exact same experience and end up stuck at the same screen as above.

![](/images/GoogleWifiAndTheCurseOfSimplication_buller.gif)

This is what we in the IT industry call a problem. At this point I'm mumbling "stupid fucking app" to myself, so I grab my laptop and an Ethernet cable. I connect my laptop to the one LAN port on the AP; sure enough, I get a DHCP address and can reach the Internet. So the AP is making a WAN connection. The mobile app just won't connect to the AP for some reason. I can check the network settings on my laptop, though, to see the local IP of the AP. While being salty at Google for designing this for the less technically inclined, I do a quick `nmap` to see if ports 80 and/or 443 are open on the AP, indicating HTTP and HTTPS, respectively.

nmap -Pn -p 80,443 192.168.x.y

80 was open while 443 was closed. I open a browser and navigate to:

https://192.168.x.y

Sure enough, this loads a web page on the device. In most consumer routers this is where you'd normally do all of your configuration. In Google Wifi it politely recommends that I get bent and use the mobile app:

![](/images/GoogleWifiAndTheCurseOfSimplication_ScreenShot2020-03-09at55533PM.png)

Shit. With the app being my **only** option, I hop back over to the setup instructions online to see what I may have missed. At this point I notice a chat pop-up at the bottom right corner of the screen for Google support. I click on it and get connected to a support agent. After I describe the situation, she instructs me to:

1. Kill the Google Wifi app on my phone.
2. Turn off cellular data on my phone.
3. Open the Wifi network settings, search for the network named on the bottom of the AP, and connect to that.
4. Re-open the app and now go through the setup.

At this point, it actually connects and gets past the screen I was stuck on so that I could configure the device. I was happy to have it working, but I was also extremely confused; nothing in the setup mentions connecting to the setup network on the device. Nothing like this was mentioned on the [common issues](https://support.google.com/wifi/topic/6261534?hl=en&ref_topic=6243113) page, either.

Regardless, with the Wifi network running, I connect a second AP to build the mesh network. I even clarified with the support agent in chat that for **this** AP, I don't connect to the setup network... I should be able to be connected to my new Wifi network, tap the option to add another AP in the app, and have it connect. Unfortunately for me, the _exact_ same thing happens as before. The AP is found, but I again sit endlessly at the "Connecting to Wifi point" screen. The support agent has me reset the AP again to no avail. Next she has me use an Ethernet cable to physically connect the mesh AP to the first AP and then repeat the setup:

![](/images/GoogleWifiAndTheCurseOfSimplication_PhotoMar092C71516PM.jpg)

Unsurprisingly, this makes no difference. On a whim, I grab my iPad, install the Google Wifi app (which doesn't actually have an iPad variant and looks like garbage), and repeat the process. The app on my iPad immediately connects and prompts me to scan the QR code on the bottom of the AP. Within two minutes the AP is connected and working.

I then head back to my bedroom where I had already plugged in the third AP during one of the instances of watching the app on my iPhone refuse to connect. It also immediately completes the setup when using my iPad.

![](/images/GoogleWifiAndTheCurseOfSimplication_dbz_rage.gif)

At this point I'm just shy of 3 hours in to something that I assumed would take me 30 minutes at most, and I _still_ have to go through the painful process of connecting my smart speakers to the new network. I'm glad it's all working, but I've got so many points of confusion.

1. Why are the setup instructions Google publishes wrong?
2. WTF is going on between the mobile app and my iPhone 11 that it wouldn't work, but it works fine on my iPad?
3. Why on Earth does Google not permit you to do configuration locally, and then have the device sync that configuration to the cloud to keep everything in sync.

#3 is a particular sticking point for me. Google only permits the configuration to sync from the cloud down to the device. Why not permit me to configure the device locally, and then make the device sync any changes to the cloud; the sync could work in both directions instead of in 1 direction. I understand wanting to leverage the app to (hopefully) simplify the process for people who would find webbing to their router way too confusing, but not permitting you to do a local configuration when you know how to connect but the shitty app doesn't work is infuriating to me.

That being said, this was 100% my own fault for not doing the research to realize that Google Wifi doesn't permit this prior to making the purchase. And to be complete clear, I'm not opposed to the app. The first setup was good, and I love being able to quickly check and configure my network from my phone regardless of where I am. I just feel like it should be mandatory to allow some degree of local access for when the app goes haywire. Unfortunately, this appears to be a standard practice. I verified that [Eero also uses an app-based setup](https://support.eero.com/hc/en-us/articles/207937603-How-do-I-set-up-eero-), and that if you don't have data on your phone, you [still need to use your phone but do some janky local connections between your old router and your Eero](https://support.eero.com/hc/en-us/articles/207921153-How-do-I-set-up-my-eeros-if-I-don-t-have-cell-data-service-). How that is less complicated than connecting a laptop and opening a browser I'll never know.

Despite my copious amounts of salt, I'm glad I finally got everything connected. As Wifi 6 becomes more prevalent, though, I'll be on the lookout for new networking hardware. This time I'll be sure to verify that whatever I buy at least offers local configuration options in conjunction with an app.
