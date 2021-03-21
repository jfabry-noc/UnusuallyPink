+++
title = "Ubuntu Linux GRUB Error After 20.04 Upgrade"
date = "2020-10-24"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["20-04", "grub", "linux", "ubuntu"]
tags = ["20-04", "grub", "linux", "ubuntu"]
+++

While I've [nuked my personal VPS](https://unusually.pink/github-pages-hosting/), I still have a VPS that I use for work; it comes in handy for things like running `cron` jobs, maintaining persistent shells, and generally handling things where a Linux shell seems better than a macOS shell (I'm looking at you, remote PowerShell sessions connecting to Microsoft Exchange.) This week I decided to upgrade it from Ubuntu 18.04 to Ubuntu 20.04. I like to stick on the LTS (long term support) releases for my servers, but I do typically prefer to keep even the LTS releases upgraded rather than waiting for them to go end of life. I _could_ have kept using Ubuntu 18.04 with [maintenance updates until 2023 and security maintenance until 2028](https://ubuntu.com/about/release-cycle), but what's the fun in that?

Upgrading a VPS is always a bit of a nerve-wracking situation just because I don't have local access to the host in case something goes extremely awry. Ubuntu tries to help alleviate this by opening a second SSH daemon on a different port just in case the primary daemon crashes during the upgrade, but if the machine ends up in a non-bootable state I'm still more or less hosed. Fortunately for me, things almost went off without a hitch... almost.

While the upgrade _did_ complete, I received an error toward the end of the process that [GRUB](https://www.gnu.org/software/grub/) failed to upgrade successfully. This was mildly terrifying since GRUB is the bootloader; if it's not working properly the system won't boot, and I can't access the host of the VPS to troubleshoot it. Luckily, GRUB continued to work in my case, and my system was able to reboot successfully after the 20.04 upgrade and beyond. GRUB just wasn't getting upgraded. I quickly noticed that I also received an error from GRUB every time I ran `sudo apt update && sudo apt upgrade` to update my system. Again, the other packages would upgrade successfully, but GRUB would always complain:

> dpkg: error processing package grub-pc (--configure):
> 
> installed grub-pc package post-installation script subprocess returned error exit
> 
> status 1
> 
> Errors were encountered while processing: grub-pc
> 
> E: Sub-process /usr/bin/dpkg returned an error code (1)

After spending some time just ignoring the problem since it wasn't exactly critical, I finally decided to do some digging. It turns out that problems like this have apparently plagued Ubuntu upgrades for a while, as I found a thread with the [same problem all the way back with an upgrade to Ubuntu 14.04](https://askubuntu.com/questions/636456/14-04-upgrade-triggers-grub-pc-failure/636523#636523). The solution in that case was to simply "nuke and pave" by removing GRUB and then re-installing it. It's once again a bit of a white-knuckle situation since if anything happens between removing and reinstalling GRUB the system will not have the ability to boot. The steps were very similar to the linked thread with some minor differences in the era of Ubuntu 20.04. The first step was still to purge GRUB:

```shell
sudo apt-get purge grub-pc grub-common
```

Running this command in 2020 removes `/etc/grub.d/` already, so there's no reason to manually run the removal. Instead, I next moved straight to re-installing GRUB:

```shell
sudo apt-get install grub-pc grub-common
```

The installation process kicks off an interactive wizard asking which disk(s) GRUB should be installed to. In my case, I only needed it on the main disk, which is `/dev/sda`. With that done, I updated GRUB and then rebooted:

```shell
sudo update-grub
sudo reboot now
```

This part kind of sucked as I was left running `nmap` against the SSH port for my VPS and hoping that GRUB was properly set up to allow the system to boot. After a nervous 15 seconds, though, the port started to respond again, and I could successfully SSH into the server. Re-checking for updates showed that everything was fine; the errors about GRUB having a needed upgrade that couldn't be installed were gone. Admittedly, it was probably unnecessary to go through this upgrade without any specific reason for it, but the beauty of Ubuntu is its popularity. Rarely will there be an issue someone else hasn't encountered, solved, and documented before, and this problem was no exception.
