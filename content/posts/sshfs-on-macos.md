+++
title = "SSHFS On macOS"
date = "2020-03-08"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["fuse", "macos", "sshfs"]
tags = ["fuse", "macos", "sshfs"]
+++

Having switched to a MacBook Pro for work when I started a new job about 6 months ago, I've been on an interesting journey in finding new software to fit my workflow after over a decade of operating primarily on Windows for my professional computing. Given that I've used Linux for **years** at a personal level, I can typically do anything I need on macOS by opening a Terminal; it's only when I've got to operate at the graphical level that I can get a little tripped up by the differences between macOS and Windows.

I recently found myself needing to copy some files from a cloud VPS running on [Linode](https://www.linode.com/). The reverse had been simple enough; copying files from my local machine to the VPS was as simple as running one `rsync` command. Note that I swap the SSH port on all of my VPS instances to something non-standard so that I don't get so many random **root** authentication attempts when nefarious people see that port 22 is open. Even though I disable root login I still want to discourage the attempts for anything that has SSH open to the Internet. If you think I'm overreacting and you have a server in the same position, just do this and see how you feel after:

```
sudo tail /var/log/auth.log
```

It's low-key terrifying. That being said, it was still easy enough to `rsync` anything from my MacBook Pro to the VPS (usually a script I had been working on that I figured would take hours to run and thus would be better suited to running on my VPS) via:

```
rsync --delete -azP -e "ssh -p #####" LocalFolder/ usere@server.domain:/local/file/path/
```

Note that the `#####` should be replaced by the SSH port number on your server if you're using something other than the default of port 22. You can verify the SSH port in use by checking `/etc/ssh/sshd_config` on your server and looking for the `Port` entry.

For me, though, this typically _only_ works in the direction of going from my local laptop to my VPS. After finishing one particularly log-running script, though, I had a file on my VPS that I needed to copy back to my MacBook. In that case, `rsync` wouldn't work because my laptop doesn't have a public IP address, and I _really_ didn't want to bother with trying to do port-forwarding for it. In the Windows world, I would've typically used something like [WinSCP](https://winscp.net/eng/index.php) to copy the files. [FileZilla](https://filezilla-project.org/) also works but, in my previous experience, has been super buggy. I tried looking in the macOS app store, and I was almost immediately disappointed. Most of the offerings either 1.) looked super shady with bad reviews or 2.) were aubscription-based applications. Don't get me wrong... I'm not particularly opposed to subscription services. In fact, [I have a podcast episode waiting to be published on this very topic](https://sameshadeofdifference.com/). But for something like a simple _file transfer_? I refuse to pay a subscription for something so basic.

As a result, I ended up looking for open-source alternatives. After just a little hunting I stumbled across [sshfs](https://github.com/libfuse/sshfs). I installed the `sshfs` base from [Homebrew](https://www.unusually.pink/podcast/episode-16-abandonment-and-homebrew):

```
brew install sshfs
```

Before `sshfs` will work, though, you also need to install [FUSE](https://osxfuse.github.io/), which I just did from their website. Note that after upgrading my device from Mojave to Catalina I needed a new version of FUSE; luckily the system is good about letting you know that your version is out of date if that's a problem.

What `sshfs` allows me to do is to mount a remote filesystem like a local filesystem that I can then use to copy files back and forth. To be extra basic, I created my mount point in my `~/Downloads` folder:

```
mkdir ~/Downloads/sftp
```

It took me some messing around to figure out the exact syntax I needed to successfully mount the filesystem for my VPS, but I eventually ended up with a shell script that just included this line:

```
sudo sshfs -o allow_other,defer_permissions -p ##### user@server.domain:/local/file/path /Full/Path/ToLocal/MountPoint/
```

Just like before, change the port number, server, and file path information to match your actual setup. After running this, you can then open Finder in macOS and easily copy files to and from the server without worry about port forwarding. The only caveat I've noticed is that sometimes macOS becomes extremely unhappy if the remote filesystem is left mounted and the local client loses connectivity to the remote system, for example because the laptop fell asleep and disconnected from WiFi. As a result, I only leave the connection up when I'm actively working with it. When I need to disconnect the mounted remote filesyste, I can either click the "eject" icon next to it in Finder or I can run:

```
sudo unmount /Full/Path/ToLocal/MountPoint
```

In this case the mount point is `~/Downloads/sftp`. I now use this setup on a nearly daily basis, and I've not run into any issues. The free, open-source solution is working great without forcing me to try out random shady-looking apps or coughing up money for a subscription just to move files between devices.
