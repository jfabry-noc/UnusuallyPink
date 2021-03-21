+++
title = "Enable Directory Listing On Specific Directory With Nginx"
date = "2021-03-04"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["autoindex", "directory-listing", "nginx", "web-development", "website"]
tags = ["autoindex", "directory-listing", "nginx", "web-development", "website"]
+++

**Note:** In the sample below, I'm using Ubuntu Linux as the host for my web server. The instructions should be the same for other flavors of Linux, though the path to your Nginx sites configuration files may be different.

Generally when you navigate to a website, such as [https://unusually.pink/](https://unusually.pink/), you don't actually get to see the files which live on the server at the location hosting all of the HTML, JavaScript, CSS, and other goodies for the site. Instead, the web server software will be configured to look for specific files in each folder, such as `index.html`, and render those. In some instances, however, you may actually _want_ to allow someone to browser the files in a directory. This was the situation I found myself in a few days ago for a skunkworks project at work. I already had a server set up running Nginx, and I just needed to expose a particular directory.

Most searches I did appeared fairly straightforward. The easiest way to do this is to define the `autoindex` parameter inside of a `location` block of the site's configuration file at: `/etc/nginx/sites-available`. I initially did that and had something that looked like this. Note that these are only the relevant pieces for this post, and this is _not_ the full configuration file:

 root /var/www/my.website.com/html;
 index index.html index.htm index.nginx-debian.html;
 server\_name my.website.com;
 location / {
         try\_files $uri $uri/ =404;
 }
 location /targetfolder {
         root /var/www/my.website.com/html/targetfolder;
         index index.html;
         autoindex on;
         autoindex\_exact\_size off;
 }

Everything in the second `location` block contained my newly added content to make this directory I had just created, `targetfolder`, exposed. After saving the file, I first tested my configuration with:

```shell
sudo nginx -t
```

Crucially, this told me the configuration syntax was okay. Then I restarted my web server via:

```shell
sudo systemctl reload nginx
```

Finally, I tried navigating to my directory via a web browser... only to be greeted by a 404 error. The 404 (which indicates that the requested resource wasn't found on the server) tells me that something is likely amiss with how I'm referencing the requested content. I compared my configuration to a handful of samples online, though, and didn't see what was wrong. Eventually, it dawned on me that all of the examples I found were showing how to enable directory listing for an _entire_ site, whereas my scenario was as little different considering I only wanted to expose a subfolder. I did a few more searches with that slant in mind, and I eventually hit upon a thread where someone mentioned that having multiple `root` definitions in their configuration file caused problems for them.

I tried adding one additional character to my configuration to comment out the `root` definition in my second `location` block:

 root /var/www/my.website.com/html;
 index index.html index.htm index.nginx-debian.html;
 server\_name my.website.com;
 location / {
         try\_files $uri $uri/ =404;
 }
 location /targetfolder {
         #root /var/www/my.website.com/html/targetfolder;
         index index.html;
         autoindex on;
         autoindex\_exact\_size off;
 }

After another `sudo systemctl reload nginx`, I was able to view the directory just like I expected. In hindsight this makes a lot of sense; if I already define the root of the site somewhere earlier in the file, I can't simply change it on a whim later on. Instead, the `location /targetfolder` directive is going to be _relative_ to the `root` I already specified. It was interesting to me that checking my configuration with `sudo nginx -t` originally didn't alert me to anything, but I'm hardly an Nginx guru. It's certainly possible there may be some scenario where the setup I had was valid.
