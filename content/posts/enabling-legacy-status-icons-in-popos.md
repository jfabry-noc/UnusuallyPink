+++
title = "Enabling Legacy Status Icons In Pop!_OS"
date = "2019-06-20"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["discord", "dropbox", "popos"]
tags = ["discord", "dropbox", "popos"]
+++

As we mentioned back in [Episode 6](https://www.unusually.pink/podcast/episode-6-huzzah-hobbies), I had installed [Pop!OS](https://system76.com/pop) on my desktop. The design of the Pop!\_OS interface is very streamlined and minimal to allow you to focus on things without as many distractions as you’ll frequently see in competing operating systems. That’s pretty awesome, but sometimes I _want_ distractions… namely the ones I get by seeing which applications I have up and running in the background. As you’re probably used to seeing, applications like [Dropbox](https://www.dropbox.com/) and [Discord](https://discordapp.com/) will drop a small icon somewhere in your OS to let you know the applications are running. For example, by default they’ll appear in the bottom right corner of the screen in Windows and at the upper right corner in macOS which is right next to the clock. In [Ubuntu](https://ubuntu.com/), which Pop!\_OS is based on, they’ll also show up next to the clock in the upper-right corner of the screen.

I could live without a Discord icon, but I really wanted one for Dropbox; it’s useful to see the icon change based on the sync status of the service. I did a little bit of digging, and the most challenging part was honestly what search terms to use in order to find the information for which I was hunting. Luckily, it didn’t take long for me to find the [official documentation](https://pop.system76.com/docs/status-icons/) on the matter.

> “Ubuntu and previous versions of GNOME Shell supported “status icons” or “AppIndicators” where installed apps could add arbitrary icons to the shell. In GNOME Shell 3.26, this functionality was removed in favor of other APIs.”

The issue isn’t that Pop!\_OS doesn’t support status icons, but that it doesn’t support _legacy_ status icons… at least not out of the box. The application **gnome-shell-extension-appindicator** from the standard repositories will fix this, though. Just install it via:

```shell
sudo apt install gnome-shell-extension-appindicator
```

Once it’s installed, launch it with:

```shell
gnome-shell-extension-prefs
```

Then turn on **KStatusNotifierItem/AppIndicator Support**. Boom. Okay, not boom. I had to log out and back in first, as noted in the documentation. After _that_, though, I was able to see my Dropbox icon in all its glory.

![](images/EnablingLegacyStatusIconsInPop_OS_window_view.png)

Worth it. Stay pink!
