+++
title = "Upgrading Star Labs Firmware On Manjaro Linux"
date = "2021-03-10"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["firmware", "linux", "manjaro", "star-labs", "star-lite"]
tags = ["firmware", "linux", "manjaro", "star-labs", "star-lite"]
+++

As you may recall, I [purchased a new Linux laptop from Star Labs](https://unusually.pink/star-lite-mk-iii/) a while back. It’s a solid little device, though I admittedly haven’t been using it as much as I anticipated due to my increased reliance on my iPads. For more information on that, look for the new [Same Shade Of Difference](https://sameshadeofdifference.com) podcast episode that should be dropping _very_ soon. That being said, because I don’t have the worst operational security ever, I do update my laptop every time I happen to see that something is pending. Given that I run [Manjaro Linux](https://manjaro.org), that tends to happen frequently. Manjaro is what’s known as an evergreen Linux distribution. It uses a rolling release cycle, meaning that there are no new “major” releases of the OS. For example, there’s no equivalent in Manjaro of moving from Ubuntu 20.04 to Ubuntu 20.10. Occasionally the maintainers spin off a new .iso with updated packages, but that’s just to save users from having to upgrade an insane number of packages when they do a fresh installation.

While I’ve been diligent about upgrading my OS and my Linux kernels (which Manjaro also provides a nice manager for), I’ve neglected to upgrade my firmware. Fortunately, Star Labs makes this process extremely simple. All of their firmware is published to [their GitHub account](https://github.com/StarLabsLtd/firmware?utm_campaign=9th%20March%202020%20coreboot%20MK%20III%20%28TQpHxp%29&utm_medium=email&utm_source=Newsletter&_ke=eyJrbF9jb21wYW55X2lkIjogIlBjaGtVQyIsICJrbF9lbWFpbCI6ICJqamZhYnJ5QHByb3Rvbm1haWwuY29tIn0%3D). There’s no need to actually keep tabs on that if you don’t want to, though, because Star Labs also sends periodic emails announcing firmware updates for their devices. I obviously can’t speak to how deep into the lifecycle of each device these may go, but at least for the time being it’s extremely helpful.

Even better, when it comes to installing firmware there’s a dedicated utility for it. They cover the [gamut of options on their support site](https://support.starlabs.systems/articles/installing-updates-from-the-lvfs?utm_campaign=9th%20March%202020%20coreboot%20MK%20III%20%28TQpHxp%29&utm_medium=email&utm_source=Newsletter&_ke=eyJrbF9jb21wYW55X2lkIjogIlBjaGtVQyIsICJrbF9lbWFpbCI6ICJqamZhYnJ5QHByb3Rvbm1haWwuY29tIn0%3D) since they support a large number of Linux variants, but for Manjaro you just need to start by installing a couple of additional packages:

```shell
sudo pacman -Sy fwupd gnome-firmware
```

I realized after the fact that I probably _didn’t_ need **gnome-firmware** since I’m unlikely to ever use the GUI for this, but it’s not as though the application takes up a large degree of space. Once the packages are installed, the next step is to check for any new firmware.

```shell
sudo fwupdmgr refresh
```

This should report back if there are new firmware updates. In my case, it reported firmware updates for 2 items, one of which was 3 revisions out of date. Yikes! I kicked off the firmware upgrade via:

```shell
sudo fwupdmgr update
```

It’s worth mentioning that the system will complain if you run the above command without having a power source connected since, generally, upgrading firmware and losing power in the middle of the process is a recipe for bricking your device. If power isn’t connected, though, it’ll very smartly prompt you to connect power and retry.

After the installation kicks off, the device will need to reboot. After the reboot, you’ll land in the firmware itself and be prompted to confirm the upgrades you’re about to apply. Use the arrow keys to move to the bottom and apply the pending changes (they’ll be pending by default.) This may take some time, and in my case the firmware application would move from 0% to 100% multiple times without any indication it was at different stages or anything like that. I recommend getting some coffee and letting your device do its thing. Once the upgrades are fully completed, you’ll be prompted to hit literally any key to kick off another reboot. This reboot will take noticeably longer than a normal reboot, and you’ll spend a decent amount of time staring at a black screen before you even see the Star Labs logo appear like you normally would during the boot cycle. Patience is a virtue, so just let the device do its thing. Eventually you should see the login screen you know and love.

At that point, just log back in and everything should be running as expected with the new firmware. If you want to be certain, running...

```shell
fwupdmgr refresh
```

... again should confirm there are no longer any pending firmware upgrades. It’s also worth mentioning that, during the upgrade process, many peripherals may end up being disconnected if the device is docked or anything like that. In my case the laptop was running standalone with nothing but AC power connected. The upgrade utility also very kindly warns you of this ahead of time, so just be on the lookout for the messages it gives you as you go through the upgrade process. The tl;dr is that if you read the output given to you by **fwupdmgr**, everything should go smoothly.
