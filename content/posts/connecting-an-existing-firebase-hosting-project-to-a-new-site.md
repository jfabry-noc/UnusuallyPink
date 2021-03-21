+++
title = "Connecting An Existing Firebase Hosting Project To A New Site"
date = "2020-09-27"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["firebase", "google", "hosting", "web-development", "website"]
tags = ["firebase", "google", "hosting", "web-development", "website"]
+++

As a follow-up to my [last post on GitHub Pages](https://unusually.pink/github-pages-hosting/), I mentioned that I moved one of my websites to [Firebase](https://firebase.google.com/). Firebase is a platform from Google for creating web and mobile applications. As a PaaS offering, there are a **lot** of different parts to the service, but as a platform for web applications hosting is naturally one of them. The [free Spark plan](https://firebase.google.com/pricing/) offers 10 GB of storage, 360 MB of data transfer per day (which works out to 10 GB of bandwidth per month), and support for custom domains and SSL. That's more than enough for me to host a simple, single page website that's only made up of static HTML, CSS, and a single image. If anyone is curious, my site is using just 1.8 MB of storage and 15 MB of bandwidth. Note that bandwidth used divided by storage used will not be indicative of total hits due to caching, compression, etc.

I've used Firebase before, so I already had my Google account linked up to Firebase, and I even had a project still technically "live" there, though the domain had long since been shifted somewhere else. To be honest, it had been so long since I used Firebase that I almost forgot about it until I just happened to start receiving some well-timed emails from the service informing me that I needed to re-verify ownership of the domain I was using for my defunct project. I had no interest in re-verifying anything, but I did want to start hosting something new there.

The first step for hosting new content was to log in to the [Firebase Console](https://console.firebase.google.com/). Since I had already used the service, this gave me tiles of my existing projects; in my scenario, I just had a single project for my hosting. I clicked on that tile, and I was taken to a Project Overview screen. This gives me a high-level look at my project. To get to the hosting-specific functionality, though, I just had to click the **Hosting** option under the **Develop** menu to the left.

On the hosting dashboard, the first item listed contains all of the domains associated with the project. Clicking the 3 dots ... next to a domain allowed me to delete it; I removed the two entries (apex domain and www) for the domain I used previously. Then I clicked the button for **Add a custom domain**. I followed the instructions on the screen to add a custom domain; I won't document the steps here since they're directly covered through the [Firebase custom domain documentation](https://firebase.google.com/docs/hosting/custom-domain).

With everything configured on the Firebase side, I next needed to crack into the Firebase CLI to link up my local project. I opted to [install the standalone CLI](https://firebase.google.com/docs/cli/#mac-linux-standalone-binary), though you can still get it through `npm` if you prefer to roll that way. The first thing I had to do was link the CLI to my Firebase account. This is different based on whether you're going to be using the CLI from a system with a GUI or if you're doing it from a headless system you're accessing via SSH. I was using it from a headless system where I cannot pop a browser to follow the normal authentication process; as a result I ran:

```shell
firebase login --no-localhost
```

If you're running this from a system with a GUI, I believe you just omit the `--no-localhost` parameter. In the headless setup, though, this gives a Firebase URL to navigate to on another system. I copied it out of my terminal and pasted it into the browser in my laptop. This gives me an authentication code for the CLI. I copied that from my browser, pasted it into my terminal, and that linked the CLI to my account in the Firebase platform.

Since I was just moving my content from my old VPS to Firebase, I didn't have to worry about actually _creating_ a website; I already had one that was backed up in a tarball. I simply had to expand my tarball on the same system where I was using the Firebase CLI. I did this by creating a new directory for the project, expanding my tarball that had all of my site's content, and then copying that content to the project directory:

```shell
mkdir ~/laifu
tar -zxvf ~/temp/laifu.tar.gz
cp -r ~/temp/html ~/laifu
```

**Note:** If you look closely at the commands above, you'll see that after I expand the tarball I'm recursively copying not the entire directory but the `html` folder from it. This is due to the fact that my tarball is of the entire `/var/www/laifu.moe/` directory that Nginx was previously hosting on my VPS, and the `html` directory is what contains the content of the site. If your backup is storing the content directly (e.g. it's not in a subfolder) that's fine. However, you'll want to make a new folder inside of your project directory that you copy the content to because you do _not_ want the content of the site to be in the root of the Firebase project's directory. For example, your `mkdir` command would look something like: `~/myproject/html`

One I had the files situated accordingly, I needed to tell Firebase that my directory was a Firebase project. Similar to using `git`, I do this by navigating to my project directory and running:

```shell
firebase init
```

This gets the ball rolling by asking some questions interactively through the CLI. One question will ask what service the project should be connected to; be sure to pick "Hosting." After that there should be a prompt for which existing hosting project you'd like to use. The existing project should be listed as an option to be selected. If it's not there, you can cancel out of the process and ensure everything worked correctly with your authentication by running the following and verifying that you see the project. If it's missing, you may need to redo the authentication (e.g. maybe you were in the wrong Google account when pasting into your browser.)

```shell
firebase project:list
```

After selecting the project, the CLI will ask what to use as the "public directory." This is essentially asking what directory _inside_ of the project directory contains the web content to be hosted. In my case I picked `html` since that's what I named the folder.

Be wary of the next couple of prompts, which will trigger regardless of whether or not there's something in your public directory matching them. When prompted about your `404.html` page, opt not to overwrite it unless you really hate your existing one. When prompted about `index.html`, **definitely** don't overwrite it or you'll lose the first page of your site.

Once that's all done, you should get a message:

> “Firebase initialization complete!”

This means that the directory has been initialized successfully as a Firebase project, but the local content still hasn't been pushed to the cloud. So the last step is to run the following:

```shell
firebase deploy
```

This will give a "Deploy complete!" message along with a Firebase-specific URL in the format of:

https://project-name-GUID.web.app

Copying this URL and pasting it into a browser should allow you to verify that the content you expect is now being hosted, even if you're currently waiting for DNS TTLs to expire before you can navigate to the custom DNS. The Hosting Dashboard of the Firebase console will also show the update in the "Release History" section.
