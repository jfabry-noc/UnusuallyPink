+++
title = "macOS Big Sur 11.2 Homebrew and Git Error"
date = "2021-02-15"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["big-sur", "brew", "git", "homebrew", "macos"]
tags = ["big-sur", "brew", "git", "homebrew", "macos"]
+++

After I upgraded my macOS install from 11.1 to 11.2, the next time I tried running `brew update` to update all of the packages available in [Homebrew](https://brew.sh/), I got a wall of text regarding things going off the rails that ultimately culminated with this error:

> Error: Failure while executing; `git config --replace-all homebrew.analyticsmessage true` was terminated by uncaught signal KILL.

When this happens, not only is Homebrew broken, but `git` itself is broken. Running a simple `git status` inside of an existing repository will similarly result in an error. I initially tweeted about it after I found a [relevant thread on GitHub](https://github.com/Homebrew/discussions/discussions/673).

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Homebrew was fubar on my M1 MacBook Pro after I installed macOS 11.2 last night, but it was simple to fix. Along with reinstalling the Xcode CLI tools and git, I also needed to reinstall gettext and pcre2.<a href="https://t.co/8gxlw7tdJF">https://t.co/8gxlw7tdJF</a></p>— John Fabry (@UnusuallyPink) <a href="https://twitter.com/UnusuallyPink/status/1356965779158298625?ref_src=twsrc%5Etfw">February 3, 2021</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

I did have to combine the steps from a few different support threads, though, so after I had to go through the same process again after upgrading from macOS 11.2 to 11.2.1, I figured I'd throw together a quick post about what worked for me. From the input others are sharing on GitHub, it seems different combinations are working for different people, so your mileage may vary.

The first thing that I needed to do is reinstall the Xcode CLI tools. This is _not_ a full Xcode installation, so no need to worry about a massive download. If you get the warning about being on battery and the recommendation that you only do the installation while the device is plugged in, you can likely just ignore it and proceed on battery power; on my system the install takes about 2 minutes.

```shell
xcode-select --install
```

For some people, the second and final step is to reinstall `git`. This did _not_ work for me, though. Following [another thread](https://github.com/Homebrew/discussions/discussions/439#discussioncomment-261267), I discovered that I needed to reinstall two dependencies of `git`: `gettext` and `pcre2`. I'm assuming that you can likely reinstall both with one command without any issues, but both times I've seen the problem thus far I've run the commands individually:

```shell
brew reinstall gettext
brew reinstall pcre2
```

The final step then is to _now_ reinstall `git`.

```shell
brew reinstall git
```

Once that's done, now re-running `brew update` or _any_ `git` command should be successful.
