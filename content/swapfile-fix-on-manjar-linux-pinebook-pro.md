+++
title = "Swapfile Fix on Manjaro Linux Pinebook Pro"
date = "2020-06-13"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["dd", "fallocate", "linux", "manjaro", "pinebook", "pinebook-pro", "swap", "swapfile"]
tags = ["dd", "fallocate", "linux", "manjaro", "pinebook", "pinebook-pro", "swap", "swapfile"]
+++

I wrote about a month ago of an update [on my Pinebook Pro](https://www.unusually.pink/blog/unusually-pink-impressions-pinebook-pro) after [switching to Manjaro Linux](https://www.unusually.pink/blog/pinebook-pro-follow-up-manjaro-linux). I continue to make heavy use of my Pinebook Pro while I'm still social distancing and staying at home, giving me lots of time to work on personal projects without much else to do.

Today I ran into an interesting issue, though, after applying a round of updates. I mentioned in my last Pinebook Pro post about going through the process of creating a swapfile using the [directions from the Manjaro wiki](https://wiki.manjaro.org/index.php?title=Swap#Using_a_Swapfile) so that I could hibernate the machine and get _much_ better battery life. I literally just opened the device back up, though, and saw a familiar message that there wasn't enough swap space to successfully hibernate the machine, leaving me with about 20% less battery life than I had been expecting. Running `htop` quickly showed me that my swap space was back to nothing. Weird.

Following the instructions, my swapfile is literally called **swapfile** on the root of the filesystem. I verified it was still in place with:

```
ll / | grep swap
```

With the file still in place, I ran through the instructions again. These ones were both successful:

```
sudo mkswap /swapfile
sudo chmod u=rw,go= /swapfile
```

I got an error when running this, though:

```
sudo swapon /swappfile
```

This told me:

> swapon: /swapfile: swapon failed: Invalid argument

Weird. I verified that I still had my swapfile entry in `/etc/fstab` to assign the swapfile on boot by running:

```
sudo cat /etc/fstab
```

I tried running the `swapon` command a different way to have it apply everything in the `fstab` file:

```
sudo swapon -a
```

This gave me another "Invalid argument" error still referencing "/swapfile". At this point, I took to the Internet. After a couple of searches, I stumbled across [a matching Stack Exchange question](https://unix.stackexchange.com/questions/294600/i-cant-enable-swap-space-on-centos-7). The top answer claims that the issue is the initial `fallocate` command used to create the file (the same command used in the Manjaro wiki) doesn't physically allocate the 4 GB on the storage for speed, but that's apparently problematic for `swapon`. Following that answer, I ran a `dd` command to replace my swapfile with something physically allocated:

```
sudo dd if=/dev/zero of=/swapfile count=4096 bs=1MiB
```

This takes a few moments to run, especially on the eMMC storage of the Pinebook Pro. Once it completed, though, I repeated the other commands and verified they were all successful. Sure enough, running `htop` once again shows my 4 GB of allocated swap space.
