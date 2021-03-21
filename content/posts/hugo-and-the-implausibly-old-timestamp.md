+++
title = "Hugo and the Implausibly Old Timestamp"
date = "2020-07-22"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["bash", "hugo", "shell", "tar"]
tags = ["bash", "hugo", "shell", "tar"]
+++

Management of one of my blogs is handled through a variety of shell scripts. I have a script for executing `hugo` to rebuild the site and copy the output of the `public` directory to the folder where Nginx hosts it, for example. One of my scripts creates a tarball of the site and uses `rsync` to copy it to another server so that, if my VPS blows up, I can easily retrieve the backup.

After composing [yesterday’s post](https://unusually.pink/making-a-hash-for-the-html-integrity-property/), though, I ran into an error with the backup script. It basically runs the following:

```shell
tar -zvcf /home/fail/backups/failti_me.tar.gz blog
```

This started throwing a warning:

> tar: blog/public: implausibly old time stamp 1754-08-30 16:53:05.128654848 -0550

It claimed the **public** directory where Hugo publishes the compiled site contents to was created in 1754… which is probably a bit older than seems plausible. My blog still published correctly; it was only `tar` being salty about the weird timestamp. I used `stat` to check the directory and confirmed that the timetsamp on when it was modified was completely borked:

```shell
stat blog/public/
```

That told me:

 File: blog/public/
 Size: 4096        Blocks: 8          IO Block: 4096   directory
 Device: fc01h/64513d    Inode: 512063      Links: 73
 Access: (0755/drwxr-xr-x)  Uid: ( 1000/    john)   Gid: ( 1000/    john)
 Access: 2020-07-21 18:58:28.669384769 -0500
 Modify: 1754-08-30 16:53:05.128654848 -0550
 Change: 2020-07-21 18:58:28.189382080 -0500
 Birth: -

After some searches online I found the following [GitHub issue thread](https://github.com/gohugoio/hugo/issues/6161) confirming that plenty of people other than me were seeing the same problem and that it was still present in the current version of Hugo. While I had initially been confused as to why I suddenly started seeing this now since I hadn’t upgraded Hugo or anything like that, I saw a few comments indicating that placing items in Hugo’s static directory seemed to trigger the issue; I had placed an image there from my last post, and it was the first addition to that directory in quite a while. With some additional searching and testing, I verified I could do the following to simply ignore the warning from `tar`:

```shell
tar -zvcf /home/fail/backups/failti_me.tar.gz blog --warning=no-timestamp
```

I didn’t like the idea of having such a wonky date on my filesystem; as a result, I started searching for how I could fix it by manually adjusting the “Modify” timestamp. `touch` seemed like a likely candidate, and after reviewing the [man page](http://manpages.ubuntu.com/manpages/trusty/man1/touch.1.html) I saw that there was a `-t` flag for it which would allow me to manually specify the timestamp. I basically just wanted to set it to the current time so I added the following to the my script which recompiles the site, placing it after the build and before `rsync`ing the contents of the public directory to the Nginx directory.

```shell
STUPID=$(date "+%y%m%d%H%M")
touch -t $STUPID /home/john/blog/public/
```

Sure enough, after running this the resulting `tar` command has no qualms. Likewise, re-running the `stat` command from above shows the current date and time as the modified time on the directory. I really hope this bug gets fixed soon since it seems to have been around for a hot minute, but at least I have a workaround for the time being.
