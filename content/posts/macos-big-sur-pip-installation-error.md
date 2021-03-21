+++
title = "macOS Big Sur pip Installation Error"
date = "2020-12-05"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["big-sur", "macos", "paramiko", "pip", "pip3", "python", "xcode"]
tags = ["big-sur", "macos", "paramiko", "pip", "pip3", "python", "xcode"]
+++

Given that I have technological FOMO while also being a technology masochist, I upgraded my 2018 MacBook Pro to macOS 11 "Big Sur" as soon as it was available. Common sense dictates that I should have waited a while to let others encounter and fix some of the kinks prior to upgrading myself, but what's the fun in that? I admittedly had a very smooth experience without many issues, and I've been happily on Big Sur for a few weeks now. I encountered my first big error a few days ago, though, while helping a friend with some Python code. We needed to use the [`paramiko`](https://www.paramiko.org) package, which I didn't have installed. I tried to install it with the following command:

```shell
sudo pip3 install paramiko
```

This was the first time I tried to use `pip3` to install a Python package since upgrading to Big Sur, and I was immediately rewarded with an entire screen's worth of cascading error messages. I took a little time to parse through them before hitting on something that seemed fairly key:

> clang: error: invalid version number in 'MACOSX\_DEPLOYMENT\_TARGET=11.0'
> 
> error: command '/usr/bin/clang' failed with exit code 1

It seemed that the `clang` compiler was unhappy with the fact that the targeted version of macOS was 11.0. This makes sense to me since all of the releases of macOS, back to when it was called OS X in 2001, were numbered 10.x.y. Big Sur was the first version of macOS in nearly two decades to bump the version from 10 to 11, and apparently my `clang` configuration _really_ didn't like that. Figuring that in 2 weeks since the release of Big Sur someone else had surely attempted to install some Python 3 packages, I started searching online. Plenty of responses recommended updating the Xcode CLI tools via:

```shell
sudo softwareupdate --all --install --force
```

I gave this a shot, but I simply got a message stating there weren't any updates available. I kept digging and finally hit upon a [Stackoverflow thread](https://stackoverflow.com/questions/63972113/big-sur-clang-invalid-version-error-due-to-macosx-deployment-target) with a solution that involved manually deleting the directory with the Xcode CLI tools and then reinstalling. I assumed that this wouldn't do anything since the tooling didn't see any needed updates in the first place, but on a whim I tried it. First I nuked the `/Library/Developer/CommandLineTools` directory with:

```shell
sudo rm -rf /Library/Developer/CommandLineTools
```

Once that was done, I kicked off reinstalling the CLI tools via:

```shell
sudo xcode-select --install
```

This initially estimated that it would take over 2 hours, followed by 20 minutes, followed by 2 minutes. Wildly inaccurate time estimates aside, after waiting a minute or two I had the tooling available once again. I tried to install `paramiko`, and was happily greeted with the progress indicators as it compiled along with all of its dependencies. I tried some quick code calling the package, and sure enough everything worked successfully. It's curious to me that checking for updates shows nothing, but reinstalling the Xcode CLI tools still resolves the issue. My guess is there is a configuration file somewhere that needs to be updated, and it gets both created and configured at the time of installation. This would mean I opted for the nuclear option when a bit more finesse would have sufficed, but with things working I didn't feel like trying to dig through configuration files to verify when I had code to write.
