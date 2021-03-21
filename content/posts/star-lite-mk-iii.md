+++
title = "Star Lite Mk III"
date = "2020-12-25"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["laptop", "linux", "manjaro", "star-labs", "star-lite"]
tags = ["laptop", "linux", "manjaro", "star-labs", "star-lite"]
+++

After spending some quality time with my [Pinebook Pro](https://unusually.pink/unusually-pink-impressions-pinebook-pro/), I decided that I wanted something _slightly_ more powerful for my personal laptop. While most of my heavy-lifting in the world of computers is done with my MacBook Pro for work (which was recently upgraded to a device featuring the new M1 chips; look for more on that soon both here and on the [Same Shade of Difference podcast](https://sameshadeofdifference.com/)), the Pinebook Pro's extremely weak ARM processor wasn't quite cutting it. Even doing things like writing up blog posts in the WordPress editor would occasionally lag, and watching videos above 720p is essentially impossible. I was still infatuated with the idea of a low-cost Linux laptop, though. I love using Linux, but I didn't exactly need a workhorse device from somewhere like [System 76](https://system76.com/laptops) just to write blog posts, maintain a couple of websites, watch videos, and play chess. After going down a rabbit hole of trying to find a (relatively) inexpensive Linux laptop (spoiler alert: they're rare), I decided to pre-order the [Star Lite Mk III from Star Labs](https://starlabs.systems/pages/lite-mk-iii).

## Shipping

I originally pre-ordered the laptop in the summer, with a shipping estimate of early September. I wasn't exactly in a rush, so that wasn't a problem for me. The global pandemic caused something of a cascading series of delays which ultimately meant that I didn't receive my device until mid-November. For example, production delays in China that ran behind became exacerbated when the factory closed due to [Golden Week](https://en.wikipedia.org/wiki/Golden_Week_(China)). Star Labs is based in the UK, so once production and QA were done, the devices shipped from the factory in China to the UK. There they were held up in customs for an entire week before they actually reached Star Labs in order to be redistributed to the customers who pre-ordered.

I don't say any of this to knock Star Labs or the device; it's just the nature of ordering something that needs to be shipped to the opposite side of the planet during a global pandemic. In fact, the team at Star Labs did really well in setting up a dedicated part of their website to post daily updates on the status starting in October so that everyone knew exactly what was going on. Kudos to them.

## Hardware

While it was about double the cost of the Pinebook Pro (my Lite Mk III was just over $400 USD when I ordered it), the device itself still feels premium to an order of magnitude higher than what I would've expected for the price. It came packaged up in a sleek box with a nice, blue microfiber bag around the laptop in the packaging. Another sat between the closed screen and the keyboard.

![](images/IMG_0317.png)

The body of the device is plastic (I'm pretty sure), but it feels **very** high quality and sturdy. The hinge for the screen will almost catch me off guard with how strongly it wants to close the last few degrees, but it manages to do this in a way that doesn't make it difficult to open, which is often a problem for such small, light laptops.

The trackpad is the antithesis of what the Pinebook Pro uses. It's glass rather than plastic, and it's incredibly snappy. Whereas the Pinebook Pro trackpad feels clunky and inaccurate, the Like Mk III feels precise. I've been extremely impressed with it. The keyboard, unfortunately, it's quite as glowing. I mean, it's _literally_ glowing in that it's backlit, and generally it feels good to type on with a decent bit of travel on the keys. The keys don't always feel responsive, though, and hitting them in slightly the wrong way will often result in the keys failing to register the keystroke. I notice this most commonly in the way that I hit the L and Spacebar keys, though the problem occurs on others as well. It's not a deal breaker, but it can be frustrating at times given that I type quickly and often have to go back and fix myriad errors. For the most part, I'm impressed with the amount of space they managed to give the keyboard on such a small device, though as I mentioned in my post about the [Keychron K2 V2](https://unusually.pink/keychron-k2-v2/), the dedicated arrow keys resulted in a _tiny_ right Shift key; whereas the Shift keys are normally at least double the width of the standard letter keys, the right Shift is actually **smaller** than the letter keys. Originally this resulted in my accidentally hitting the Up arrow key when I wanted to hit Shift, but after a couple of weeks I began to adjust. I'm still not perfect with it, but it's not as problematic as I had been worried that it might be. It's also a nice touch that, since the laptop is designed for Linux, it actually features Ctrl, Fn, Super, and Alt keys.

![](images/IMG_0319.png)

The display is a rather nice 1080p IPS, which has all of the benefits and drawbacks one might expect on an 11" device. While everything looks extremely crisp, I have to adjust the text size in basically every application in order to be able to read anything. My eyesight is rather poor, so your mileage may vary, but for anyone with vision problems the zoom functionality and adjustable font sizes will be essential. The speakers aren't groundbreaking, but they certainly get the job done and can be surprisingly loud.

The processor is a Pentium Silver N5000 from Intel. It's fairly low-end, but it gets the job done, and that's exactly what I needed for the price point. It'll occasionally stutter a bit for really heavy web pages or when firing up GIMP to do some light photo editing, but for the most part it chugs along happily. I was extremely impressed that the device actually comes with 8 GB of RAM, which is extremely helpful. I did [set up a swap file](https://unusually.pink/swapfile-fix-on-manjar-linux-pinebook-pro/) in order to be able to hibernate the device, but it basically stays untouched during active operation. For storage, it comes with a 240 GB over-provisioned SSD. That's configurable up to 960 GB if you need it, though I was more than fine with 240 GB.

While it's not typically something I think too much about when getting new hardware, I have to mention that the charger for the Lite Mk III is extremely nice. It's a slick-looking brick that comes with a blue and black braided USB-C cable. When I think of relatively inexpensive laptops, often the charger is an area where the OEM can save a little money, but that wasn't something they opted for in this case.

## Software

I won't touch too much on this since the experience really comes down to which flavor of Linux you want to install. Star Labs offers most major distros as pre-installed options; I went with Manjaro XFCE since I've come to enjoy using Manjaro so much on my Pinebook Pro. The experience has been great so far aside from the fact that, after immediately upgrading to the latest Linux kernel after receiving my laptop, I had horrible video issues. I simply reverted back to the latest LTS kernel (which is extremely easy to do in Manjaro), and everything was fine. Ubuntu, Mint, MX Linux, and others are all offered as options and across a wide array of window managers for each. Of course, it's simple to just reinstall any other OS whenever you want since the device is designed to facilitate that.

[Star Labs uses the LVFS to distribute new firmware](https://starlabs.kb.help/updates-upgrades/installing-updates-from-the-lvfs/), and it works very well. Along with firmware for the devices that are a part of the laptop, I've also received firmware upgrades through it for some of my peripherals that I use with it, like my Logitech MX Ergo trackball.

## Wrap-Up

![](images/IMG_0318.png)

On the whole, I'm fairly pleased with the Lite Mk III. The unreliability of the keyboard is my only major issue that gives me some headaches. Otherwise, it's exactly what I was looking for in a small, inexpensive Linux laptop. I really hope more Linux-focused OEMs consider making an entrance into this particular market for people who want something basically on the order of a Chromebook but without any Google nonsense.

I don't think I could use the Lite Mk III as my _only_ laptop, but for any Linux nerds like myself who are looking for a supplementary device it makes for a great fit. Add in the fact that the warranty allows for the device to be opened by the user and repairs made without voiding it -- the [company even offers a disassembly guide](https://starlabs.kb.help/star-lite-mk-ii/star-lite-mk-ii-complete-disassembly-guide/) -- and there's a lot to like.
