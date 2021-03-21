+++
title = "Swapping Mirrors On Manjaro Linux"
date = "2021-03-12"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["linux", "manjaro", "mirror", "software"]
tags = ["linux", "manjaro", "mirror", "software"]
+++

One of my favorite aspects of Linux ever since my first Ubuntu 6.06 installation has been the software repository. Back in 2006, installing the overwhelming majority of the software I might need from a supported repository stood in stark contrast with the Windows XP method of searching the web for the software you wanted, finding a download link that didn't _seem_ shady, and then hoping for the best.

Repositories are frequently hosted by the maintainer of a Linux distribution (e.g. [Ubuntu](https://ubuntu.com/) has official repos hosted by [Canonical](https://canonical.com/), [Fedora](https://getfedora.org/) has official repos hosted by [Red Hat](https://www.redhat.com/en), etc.) That being said, often times repositories are mirrored by service providers (e.g. [Linode](https://www.linode.com/) hosts many mirrors so that repository content can be served out of the same datacenter as the virtual machines), Linux interest groups, and other community members. This is great because it allows for the repos to be geographically diverse. Pulling downloads from a repository hosted in the same continent, for example, will typically show a marked improvement over using one on the opposite side of the planet.

The downside to this is that repos occasionally become unavailable for whatever reason. This happened to be recently, as I went to update one of my [Manjaro](https://manjaro.org/) installs via:

`sudo pacman -Syu`

After sitting for an unusual amount of time, I was told that a connection couldn't be established to my current mirror, which happened to be: **us.mirrors.fossho.st**

It's worth noting that, at least on Manjaro, your mirror is only displayed when the connection fails. If the connection is successful, you only see the results of the update check. I could have simply tried again later, but without knowing how long the outage would be I instead decided to hunt for how to swap to a different mirror. Fortunately, the process is extremely easy. The current mirrors are contained in the file:

`/etc/pacman.d/mirrorlist`

The file contains a list of mirrors, with a comment above each indicating the country. At the very top of my list was my current mirror of **us.mirrors.fossho.st**. I simply added a `#` to the start of the line for it, commenting it out. I next ran a full package sync to make sure I didn't have anything cached locally that wouldn't be reflected on a different mirror:

`sudo pacman -Syyu`

This picked a different mirror, and everything worked as expected. The problem was solved. Not satisfied with leaving things at that, though, I decided to see if I could tell of any indicators of a mirror being down. I first confirmed that I could ping **us.mirrors.fossho.st**. Likewise, I could connect to it over port 80 (HTTP.) However, I couldn't connect to it over port 443 (HTTPS.) I don't actually know what mechanism is used for deliver updates, but I feel like it's _very_ likely that HTTPS is used; I couldn't see HTTP being the norm given that you wouldn't be able to ensure the updates hadn't been tampered with. A random port is always possible, though.

For the sake of comparison, I opened `/etc/pacman.d/mirrorlist` back up and searched for another mirror in the US. **mirror.dacentec.com** was the first hit. I repeated the same process and confirmed that I _could_ reach it on both ports 80 and 443.

![](/images/nmap_mirrors_full.png)

It seems like 443 is the key. It was only after doing this, though, that I realized a project like Manjaro likely has a status page for their mirrors. [I wasn't disappointed.](https://repo.manjaro.org/) Checking this did appear to indicate that my original mirror is experiencing issues. Incidentally, the baseline mirror I tested is included in the same screenshot, and it's shown to be up.

![](/images/mirror_status-1024x139.png)
