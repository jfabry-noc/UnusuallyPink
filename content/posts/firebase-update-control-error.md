+++
title = "Firebase Update Control Error"
date = "2019-04-08"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["firebase", "web-development"]
tags = ["firebase", "web-development"]
+++

One of my websites (not this one) is hosted via [Firebase](https://firebase.google.com/). It’s a largely static site that I rarely need to touch. I manage it from [their CLI](https://firebase.google.com/docs/cli/) running on a VPS that I do some coding on so that I can access it regardless of which of my numerous devices I happen to be using. Since I don’t touch the site regularly, though, the Firebase tools tend to get a bit out of date. I needed to push a minor change the other day and figured I’d check for an update:

```
sudo npm install -g firebase-tools
```

Instead of completing happily, though, I got the following:

```
npm ERR! path /usr/local/bin/firebase
npm ERR! code EEXIST
npm ERR! Refusing to delete /usr/local/bin/firebase: ../lib/node_modules/firebase-tools/bin/firebase symlink target is not controlled by npm /usr/local/bin
npm ERR! File exists: /usr/local/bin/firebase
npm ERR! Move it away, and try again.
```

I’m a bit embarrassed that I did a bunch of super unnecessary troubleshooting at first instead of just _reading_ the error. When I finally got to that point because things like clearing the `npm` cache didn’t work, I saw noticed this:

```
File exists: /usr/local/bin/firebase
Move it away, and try again.
```

Okay, seems sensible enough. I first just renamed it in the same directory.

```
sudo mv /usr/local/bin/firebase /usr/local/bin/firebaseBKP
```

I re-ran the `npm` installation command, and sure enough it worked without any issues. I verified I could actually see `firebase` in my `$PATH`:

```
which firebase
```

And that it was the newer version:

```
firebase --version
```

With that out of the way, I simply deleted the file I renamed:

```
sudo rm /usr/local/bin/firebaseBKP
```

Then I could push the update to my site without any issues. To be honest I’m not entirely sure why or how that file wouldn’t be controlled by `firebase` or it couldn’t be removed by running the command under `sudo`, but I’m happy that it had a clear error message that allowed me to fix things easily enough… once I actually, you know, _read_ the error message.
