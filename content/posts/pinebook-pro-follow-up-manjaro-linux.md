+++
title = "Pinebook Pro Follow-up: Manjaro Linux"
date = "2020-05-17"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["arm", "linux", "manjaro", "pinebook", "pinebook-pro", "xfce"]
tags = ["arm", "linux", "manjaro", "pinebook", "pinebook-pro", "xfce"]
+++

I had [written in early February about my Pinebook Pro laptop](https://www.unusually.pink/blog/unusually-pink-impressions-pinebook-pro) after roughly a month of using it. Now that significantly more time has passed, time which I've almost exclusively spent at home due to nationwide stay-at-home orders and social distancing, I figure I'd share some of my thoughts on the laptop I raved about initially, and more specifically what I've done with the operating system.

## The OS

If you read my original post then you may recall that I was debating swapping the default Debian operating system for something different. While I mentioned NetBSD in the post I ended up installing [Manjaro Linux](https://manjaro.org/) instead. Why? There are a few reasons to consider.

One of the biggest reasons why swapping the OS can be considered important is that there are some issues with the Debian build that came on my device. It's a [custom setup put together by a community member](https://github.com/mrfixit2001/debian_desktop). It looks incredible, but that comes at a cost; the highly customized environment means that many things in it _cannot_ be updated; this includes both Chromium and Firefox. Running outdated browsers isn't exactly cool, and I wasn't too keen on having to compile Firefox from source every time there was a new release.

On the other hand many of the community members seemed to really love Manjaro Linux, so I figured I'd gvie it a shot. Along with people in the Pinebook community raving about it, it's also been hovering near the top of [Distrowatch](https://distrowatch.com/) for a while now. In fact it was [announced in March](https://www.pine64.org/2020/03/15/march-update-manjaro-on-pinebook-pro-pinephone-software/) that Manjaro was going to be the default OS for new Pinebook Pro devices, which is pretty slick! They're shipping with the KDE variant, though, and while I've read about people having a good experience with that, I personaly prefer the more lightweight [XFCE desktop](https://www.xfce.org/). It's lighter, simpler, and overall I feel like it's a better fit for the Pinebook Pro.

## Getting The OS

As something of a noob when it comes to working with open source SoC setups like a Rock64 or Raspberry Pi, I struggled a little with figuring out exactly how to install a new OS on the Pinebook Pro. The first thing I figured out is that, currently, you cannot boot the Pinebook Pro from USB. I made a few bootable flash drives like I would've historically used when installing Linux on a more traditional device without success. The [Pinebook Pro wiki](https://wiki.pine64.org/index.php/Pinebook_Pro) seems to contradict itself on this point, saying:

> The Pinebook Pro is capable of booting from eMMC, USB 2.0, USB 3.0, or an SD card. It cannot boot from USB-C. The boot order of the hard-coded ROM of its RK3399 SoC is: SPI NOR, eMMC, SD, USB OTG.

Then in the very next paragraph it says:

> At this time, the Pinebook Pro ships with a Debian + MATE build with uboot on the eMMC. Its boot order is: SD, then eMMC. Booting off USB storage is not currently available, but will be in the future.

I wouldn't put it past myself to just be misunderstanding something, though, so your mileage may vary. As you might be able to guess from the quotes shared above, I just ordred a cheap microSD card online and used that for the OS installation; everything worked great.

For Manjaro specifically, the best way to get the proper image is to go to the forum announcement for the latest version, like [this one for the 20.04 release](https://forum.manjaro.org/t/manjaro-arm-20-04-released/133374). There you can select the appropriate link for your Pinebook Pro and for your preferred desktop environment. Once you select that, you'll be taken to another page [like this one listing all of the images files you can download for that version and desktop environment](https://osdn.net/projects/manjaro-arm/storage/pbpro/xfce/20.04/).

Be aware that if you want to install Manjaro **directly on the laptop**, be sure to grab the image file with "emmc-installer" in the file name! This is the image designed to be flashed onto the internal eMMC storage of the Pinebook Pro! Grabbing the image without this will simply create a microSD card that will run the OS. This isn't bad if you'd like to try out Manjaro while leaving the Debian installation on your eMMC, but I wanted to go all-in.

## Preparing Your Media

To prepare the media, you can use basically any option that'll allow you to flash the .img file you downloaded onto the microSD card. You can be old-school and use `dd` from the command line, but I went easy mode and used [balenaEtcher](https://www.balena.io/etcher/) instead.

## The Rest

The boot order on the Pinebook Pro automatically has the microSD card slot above the eMMC, so once you have a microSD card prepared just insert it into the Pinebook Pro, boot the device, and follow the prompts; it's honestly pretty simple to install. The only catch is a known bug with the 20.04 release where you have to hit `Esc` while looking at the Manjaro logo after booting from the microSD card before it'll move forward; as a known bug I would imagine this won't be relevant for future releases.

> When the Manjaro logo has been present for about 15 seconds, press ESC (this is a bug, we know).

Once it's all done, you boot to this gorgeous view!

![](/images/PinebookProFollow-upManjaroLinux_manjaro_pbp_xfce.jpg)

## Caveats

For the most part, Manjaro works like a complete dream, and I've been extremely happy with it. There are just a couple of caveats that might be worth mentioning, though.

### Initial Update

First, there was a bug when running the initial software update on the factory image. First I had to figure out _how_ to update it, but after some quick DuckDuckGo-fu (since I've really only used Debian-based flavors of Linux before) I figured I could update Manjaro via:

```shell
sudo pacman -Syyu
```

Note that you can aslo just click the little shield icon in the bottom-right corner of the screen. Regardless of method, though, immediately after booting my new OS I would get the following error when attempting to update:

> Conflicting files: nss /usr/lib/p11-kit-trust.so already exists in filesystem

Another benefit to choosing Manjaro is that it has an extremely active community, so it's easy to search on errors like this and get solutions! I quickly found a [YouTube video](https://www.youtube.com/watch?v=NexyTfO0iuY) covering this error and providing the solution:

```shell
sudo pacman -Syyu --overwrite /usr/lib*/p11-kit-trust.so
```

After that initial update succeeded I've been able to perform subsequent updates without needing the `--overwrite` parameter. Updating via the GUI also works.

### Swap

It's entirely possible I just made a mistake in my installation, but I eventually realized that I didn't have a swap partition or file. I realized this because I noticed that the battery drain while under suspend was a bit severe; I was losing roughly 6% of my battery per hour. This meant that leaving the device on suspend for a day would completely drain a full battery. I changed my XFCE setting so that when closing the lid of the device it would hibernate. This gave me an error, though, that I didn't have enough swap space to hibernate.

I checked the output of `htop` and realized that I didn't have enough space because I didn't have **any** swap. Luckily for me, the Manjaro folks have a wiki page dedicated to exactly this; I ended up following the steps to [create a swapfile rather than adjusting my partitions to have a swap partition](https://wiki.manjaro.org/index.php?title=Swap#Using_a_Swapfile). Since there are 4 GB of RAM in the Pinebook Pro, I ended up creating a 4 GB swap file. Once it's created you can verify it by looking at `htop` or running `swapon`. If you're concerned about the amount of storage on the eMMC, you can see how much you're using via:

```shell
df -H | grep /$
```

### Typing and the Touchpad

One thing I've noticed on occasion is that typing does not disable the touchpad on the laptop, meaning that it's easy for your hand to accidentally brush it, clicking somewhere else and causing a bit of havoc on whatever it is you're trying to do. I've accidentally done it once just while working on this article. I've not found a solution for this yet (I'll update the post if I do), but it hasn't been a big deal for me. It's just something to be mindful about.

## The End

On the whole, I'm _extremely_ pleased with my switch to Manjaro. If you're running a Pinebook Pro with the original Debian build, I'd highly recommend giving Manjaro a shot; it honestly feels like a whole new device afterward.

Keep the [open source love alive](https://www.unusually.pink/blog/its-all-about-open-source), and stay pink!
