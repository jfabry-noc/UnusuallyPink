+++
title = "Unusually Pink Impressions: Pinebook Pro"
date = "2020-02-09"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["debian", "linux", "open-source", "pine-64", "pinebook-pro"]
tags = ["debian", "linux", "open-source", "pine-64", "pinebook-pro"]
+++

In my [last post about not using Chrome OS](https://www.unusually.pink/blog/rip-chrome-os), I neglected to get into _what_ I was replacing my Chromebooks with since that seemed like a lengthy post in its own right. As the title gave away, my switch went to the [Pinebook Pro](https://www.pine64.org/pinebook-pro/) from Pine 64. I had been interested in the original $99 USD Pinebook when that came out a couple of years ago, but it seemed a bit **too** basic for what I wnanted. The Pinebook Pro seemed to offer a more compelling experience for $199. The description on the site even seemed to match exactly what I was wanting.

> The Pinebook Pro is meant to deliver solid day-to-day Linux or \*BSD experience and to be a compelling alternative to mid-ranged Chromebooks that people convert into Linux laptops.

I didn't want to have to deal with Windows, nor was I wanting to go through a lot of research to find a Windows laptop or Chromebook that I could then convert to a Linux laptop. The Pinebook Pro seemed to take care of this for me while also giving me an outlet to support a great company doing some awesome things for the open source community. I was a little leery on what kind of performance I would get out of an ARM processor and if I would hit any compatibility issues, but so far everything has been buttery smooth.

# Hardware

The hardware is absurdly nice for a $199 device. The Magnesium alloy body just feels solid and sturdy. It has enough heft to make the laptop feel more expensive than it is without being a pain to carry around. It also just looks sleek given that there's literally nothing on the top, not even branding. It's a spotless, black surface.

![](images/UnusuallyPinkImpressionsPinebookPro_IMG_0051.jpeg)

I don't normally leave my laptops completely unemblazoned, so I just ordered some stickers for it earlier today. I'll share pictures on the socials once those are on.

Under the hood is the a screen that you almost feel has no business being on a $200 machine. It's a 14" 1080p IPS display that looks absolutely gorgeous. Everything from typing away in a Terminal or blog post to watching videos looks terrific on it.

The keyboard is also surprisingly nice. It's spacious, and the keys have an impressive amount of travel to them. Given that these days I rarely use my personal laptop unless I'm doing a lot of typing where I need a physical keyboard, the keys on the Pinebook Pro really deliver. Rarely do I even bother to sit at a desk and connect an external keyboard; the default experience is just fine. The keys have good tactile feedback to go along with that travel distance, so you're never left doubting if you pressed a particular key or not.

![](images/UnusuallyPinkImpressionsPinebookPro_IMG_0052.jpeg)

You may notice that the keyboard also features literally the only external branding other than the sticker on the bottom of the device. The super key has a Pine 64 logo on it, which is a nice, subtle touch.

![](images/UnusuallyPinkImpressionsPinebookPro_IMG_0050.jpeg)

The trackpad is unfortunately not quite as solid as the keyboard. It doesn't feel particular sensitive and the pointer come across as "floaty". Hitting big buttons is fine, but it can be annoying to make more precise movements like grabbing the corner of an application to resize it. If I'm using the device for something that's mouse-intensive, I'll typically reach for my [Logitech MX Ergo trackball](https://www.logitech.com/en-us/product/mx-ergo-wireless-trackball-mouse), but the trackpad is still workable in a pinch.

The speakers aren't bad, but they're about what you expect from laptop speakers. Only Apple seems to posess the voodoo required to make great sounding laptop speakers.

The device comes with a barrel jack charger. You can also charge it via the USB-C port as well, which is what I much more commonly do with the charger which came with my Acer Chromebook 315.

# Software

Once the device is booted, you'll be using a customized Debian Linux version that comes from the factory.

Distributor ID:    Debian
Description:    Debian GNU/Linux 9.12 (stretch)
Release:    9.12
Codename:    stretch

Being that it's Debian Linux, you can do pretty much anything else that you'd expect with Debian Linux... and that's **awesome**. There are some limitations in the packages simply regarding what software supports ARM and which software does not, but everything I've tried to do so far has worked fine. That being said, I'm mainly still using the device very similar to a Chromebook; I want a web browser and a Terminal.

![](images/UnusuallyPinkImpressionsPinebookPro_Screenshotat2020-02-0915-17-28.png)

The ARM processor and included 4 GB of RAM do an excellent job of keeping everything snappy. It'll occasionally take a little bit longer to launch a heavier application, but otherwise it's often easy to forget that I'm on a $200 Linux laptop and not my $2000 MacBook Pro for work. The 64 GB of local storage is a nice change from most Chromebooks as well. Rarely is my system ever bumping its head against the hardware, even when I'm streaming music and have a bunch of browser tabs open.

![](images/UnusuallyPinkImpressionsPinebookPro_image-asset.png)

Need to install something new? Just `sudo apt install package-name` and you're good to go! That being said, just like normal with Debian you get a wide array of software out of the box... all of which still only amounts to a 5 GB footprint in what comes from the factory.

# To Do

There are a few things to do out of the box when you get a Pinebook Pro; I would **highly** recommend reading [the wiki page](https://wiki.pine64.org/index.php/Pinebook_Pro) about the device so you know what those are instead of trying to do things on your own. For example, there's no traditional "first run" experience to set things up given that this is a highly customized Debian image. Instead, you log in with the default credentials `rock/rock` to get things started. It's highly recommended you [rename the `rock` account](https://wiki.pine64.org/index.php/Pinebook_Pro#Initial_user_changes.2C_password.2C_name.2C_etc) if you want to keep all of the customizations. If you make a brand new account, which is totally an option if you want to go that route, you'll end up in a session that looks much more like a traditional Debian installation.

While the Wiki does an awesome job of covering what to do when you get the device, my recommendations are:

- Disable the root account.
    - `sudo passwd -l root`
- Install and enable `ufw` for a firewall.
    - `sudo apt install ufw`
    - `sudo ufw enable`
- Run the [system update script](https://wiki.pine64.org/index.php/Pinebook_Pro#Default_Debian_MATE_Desktop_Quick_Start).
- Update the rest of the system.
    - `sudo apt update && sudo apt upgrade`
- Update the [keyboard and trackpad firmware](https://wiki.pine64.org/index.php/Pinebook_Pro#Trackpad).
- Check your keyboard version to make sure it's on the correct language. Mine came as UK English.
    - If that's not correct, type `keyboard` at the menu launcher, open the keyboard preferences, go to the `Layouts` tab, and select the apporpriate layout.
- Verify if your timezone is correct or needs to be updated.
    - `tzselect`

Of course, all of this assumes you're going to keep rolling with the factory Debian image. There's a [wide array of operating systems you can choose from](https://wiki.pine64.org/index.php/Pinebook_Pro_Software_Release). I'm personally super tempted to give the [NetBSD image](http://www.armbsd.org/arm/) a shot.

# Wrap Up

Overall, I'm _extremely_ pleased with the Pinebook Pro so far. They've got a great community, and I've spent a little time so far keeping tabs on their IRC server and Discord. If you can't find something in particular on the wiki (which would be surprising; it's extremely well-maintained), the helpful folks on IRC can likely assist. If you're a fan of budget laptops but want to move away from Chrome OS or Windows, the Pinebook Pro seems like a great option if you feel comfortable with Linux.
