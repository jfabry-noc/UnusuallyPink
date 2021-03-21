+++
title = "RememBear Password Manager"
date = "2020-11-28"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["dropbox-passwords", "password-manager", "remembear", "tunnelbear"]
tags = ["dropbox-passwords", "password-manager", "remembear", "tunnelbear"]
+++

A few weeks ago I received an email offering to let me try the premium variant of the [RememBear](https://www.remembear.com/) password manager for a year for free. I assume that I received this since I currently have an active TunnelBear subscription that I use for my VPN. I didn't bother looking into things too closely, but my understanding is that you can normally use RememBear for free, but syncing your passwords between multiple devices requires a premium subscription... meaning that in 2020 when people have a phone, a tablet, and at **least** one computer, a premium subscription is basically a requirement. I figured that I may as well give it a shot in order to see if it could possibly tide me over until [Dropbox Passwords](https://unusually.pink/dropbox-passwords/) is a bit more mature; a year of free service seems like a good bit of time for the Dropbox team to improve upon their product.

The RememBear apps for macOS, iOS, and iPadOS are all very slick. They feature the same cute, cartoon bear as TunnelBear. If I type in my master password on macOS, the bear will even move its head to follow along as I type. That being said, typing in my master password happens infrequently since the apps on all 3 platforms work really well with Touch ID and Face ID.

The apps on all 3 platforms also do well with auto-filling passwords for me, including with the browser extension for Safari on macOS. Likewise, the iPadOS app is, mercifully, an iPad application rather than the expanded iOS app. I did run into a few random bugs on macOS where the application would be blank after unlocking it, showing as if I didn't have any logins stored. I saw another issue where my logins were all listed, but using the search feature wouldn't return any results even if there were matches. In both cases, closing out of the application and re-opening it fixed the problem, and I only encountered each bug once.

![](images/remembear.png)

Adding new devices or recovering your account is streamlined with a QR code mechanism that leverages another device which already has RememBear configured on it. This makes the setup quick and easy, though it remains unclear what recourse there is if the master password itself is lost and no other devices are configured. They don't give you anything like the secret word list for Dropbox Passwords or the secret key from 1Password.

One of the nice benefits to RememBear over the immaturity of Dropbox Passwords is that it does offer an option for creating and syncing secure notes. I frequently use these for things like saving WiFi pre-shared keys. Unfortunately, I've had to start using these for saving API keys, too, because RememBear doesn't offer the option to add multiple credential fields for each login. I can also store them, much as I did with Dropbox Passwords, in the provided notes section for each login, though it does make copying and pasting the keys a bit more annoying than in something like 1Password or Bitwarden which simply give you the option to add multiple credential fields.

The biggest issue with Dropbox Passwords, though, is unfortunately shared with RememBear: there is no web-only option. Installing the browser extension for Firefox, for example, will not work on my Manjaro Linux laptops because the desktop client is still expected and there is no Linux application. This was really surprising to me since one of the reasons I initially started using [TunnelBear as my VPN service was due to the fact that they offered a standalone browser extension that I could use back when I had Chromebooks](https://unusually.pink/chromebooks-vpns-and-you/) which couldn't install any type of full-fledged VPN client. Given that RememBear has been around for at least a few years and is by no means a new product, I did a little bit of digging to see if there were any plans to support Linux or Chrome OS (while I no longer use Chromebooks because Google is pretty gross, Chrome OS support would indicate a standalone browser extension.) The latest I could find was a comment from May of 2019 on the [Chrome extension](https://chrome.google.com/webstore/detail/remembear/ehpbfbahieociaeckccnklpdcmfaeegd) which simply confirmed Linux and Chrome OS weren't supported.

With its slick, cute, and bear-laden UI, RememBear is probably one of the nicest-looking and most user-friendly password managers around. For the overwhelming majority of people, it also likely ticks all of the boxes they would care about as far as features are concerned. Any Linux users out there, though, will be disappointed with the complete inability to use it with their operating system of choice. Here's to hoping for web support or a standalone browser extension at some point in the future, but for the time being Linux aficionados are better off sticking with something like 1Password or Bitwarden.