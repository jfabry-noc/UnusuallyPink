+++
title = "Self-Hosting A Static Website"
date = "2020-07-09"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["gohugo", "hugo", "lets-encrypt", "nginx", "static", "website"]
tags = ["gohugo", "hugo", "lets-encrypt", "nginx", "static", "website"]
+++

Earlier this week a friend reached out to me regarding a website. He had just finished developing his very first iOS game and was ready to submit it to Apple for approval. One of Apple's myriad requirements, though, is a website containing the author's privacy policy. My friend had no website and no idea how to make one, so he asked me if I could help. It seems wild to me that someone could have the chops to make an iOS app in Objective-C or Swift but not be able to make a website, but each of us has a different skill set.

We first took some early steps gathering requirements. What did he want for the site? Literally just the privacy policy. Where did he want to host it? Wherever was the cheapest. Did he have a domain name already? Yes! This was fairly straightforward; he literally _just_ wanted the very basics. After a bit of discussion I convinced him to write up a quick "about me" type of page so that we could have more than just the privacy policy. From there I could get to work.

# Hosting

The first thing I did was have him head over to [Vultr](https://www.vultr.com/) and spin up their cheapest instance. I think this is running him $5 USD per month. I had him pick [Ubuntu](https://ubuntu.com/) as the server operating system given that it's the one I'm most familiar with. My friend has some familiarity with Linux but not a lot of practical knowledge; when I asked him to shoot me some SSH credentials with `sudo` access he literally sent me the root account from Vultr. Ick.

# Configuring The Host

### Accounts

My first goal was to configure the host. I started that off by creating user accounts for each of us:

```shell
adduser username
usermod -aG sudo username
```

After switching users and verifying my new account worked, I disabled root's ability to log in:

```shell
sudo passwd -l root
```

### Ports

Next I wanted to change the default SSH port since having 22 open means a million places from across the planet are going to throw garbage traffic at your server. I did this by modifying the SSH config at `/etc/ssh/sshd_config`, finding the line with `#Port 22`, uncommenting it, and changing the port to a high number of my friend's choice. Then I restarted SSH:

```shell
sudo systemctl restart ssh
```

### Firewall

I wanted to enable the firewall as well, so I opened up with the new SSH port _and_ 80 and 443 for our eventual website:

```shell
sudo ufw allow sshPortNumber/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### Webserver

I next needed a web server; [Nginx](http://nginx.org/) has been my go-to choice for a long time. Rather than re-hashing all of the steps, I'll just recommend following the excellent documentation from [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04) which nicely covers the Nginx configuration. That takes you to the point where you are hosting a website. Then you just need content on it.

### Certificate

I'm an advocate of using HTTPS for everything, and with free certificates from [Let's Encrypt](https://letsencrypt.org/) there's no reason not to. Given that we have shell access, using `certbot` is the way to go. There's also excellent [documentation on that process on Ubuntu with Nginx](https://certbot.eff.org/lets-encrypt/ubuntufocal-nginx). I highly recommend selecting the option to redirect any HTTP traffic to HTTPS.

# Website

Now for the website itself. I'm not really much of a web developer, and I dislike making anything frontend; I don't exactly have the best design sense. So I once again opted to leverage [Hugo](https://gohugo.io/) to take care of that for me. I've written about [the specifics of using Hugo in detail](https://failti.me/posts/hugo_setup_guide/). Since we really just wanted a generic landing page with my friend's socials and then links to the About and Privacy Policy pages, I ended up going with the [Hermit theme](https://themes.gohugo.io/hermit/). It has a nice, simple look. My friend's favorite color is mint green, so the default background also works nicely with that when I changed the accent color. The theme nicely includes an [exampleSite](https://github.com/Track3/hermit/tree/master/exampleSite) so that I can steal their `config.toml` file and also their "About" page to make things even easier for myself.

# Backups

One of the nice things about Hugo is that, since everything is a simple text file, it's very easy to compress your entire site and save a backup. Then if something terrible happens to your server, it's extremely easy to get the site back up and running on a different machine. In this case, I made tarballs for both the finished, compiled site and the Hugo directory storing the configuration and Markdown.

```shell
tar -zvcf ~/temp/html_output.tar.gz /var/www/mySite.com/
tar -zvcf ~/temp/hugo_directory.tar.gz /var/www/mySite.com/
```

With the tarballs created, I used an SFTP client to copy them off the server for safe keeping.

# Wrap Up

In total it took me about an hour and a half to get everything up and running. Having gone through this process _many_ times for websites of my own, I've got a decent bit of experience with the process, but this shows it still doesn't necessarily take a super long time to get a decent website up and running. The big benefits are:

1. The site is _cheap_ to run. Even the smallest instance at any VPS provider will be able to handle multiple sites with ease unless they start getting really popular, so if my friend wants to create any other sites in the future he won't need additional hosting.
2. Backups are stupid simple. My friend isn't beholden to a hosting provider or trying to work within the confines of something more expensive like Wordpress or Squarespace.

The downsides are present, though, so you have to be cool with them:

1. Setup takes more technical chops than clicking through a Squarespace template editor. While the documentation for everything in this post is extremely good, if working out a terminal freaks you out then this likely isn't for you.
2. Content is authored in Markdown. This likely doesn't matter for my friend at the moment since he's not really posting anything new to the site, but it would be something to keep in mind if he decided to start a blog. In that scenario, I usually just SSH to the server and author my content in [Vim](https://www.vim.org/). You could also author the Markdown elsewhere and copy it to the server, or use SFTP to open the Markdown file on the server from an editor on your local machine. It's definitely _not_ as simple as a WYIWYG editor in your browser, though.
3. Maintenance is something that will need to be done at least periodically. The server will need to be patched. That's easy enough to do with a simple `sudo apt update && sudo apt upgrade` and then reboot when necessary, but it's just another step to keep in mind. Likewise, bouncing the server means that the website will be down, even if it's typically only for a moment or two.

Being kind of pretentious, technical snob I personally find it _easier_ to author my comment in Markdown on Vim instead of using a WYSIWYG editor in a GUI, but your mileage will vary based on your own prefrences.
