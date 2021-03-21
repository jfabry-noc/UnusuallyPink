+++
title = "Fixing Let's Encrypt Certificates After You Delete Them Like An Idiot"
date = "2019-07-22"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["https", "lets-encrypt", "linux", "nginx", "ssl"]
tags = ["https", "lets-encrypt", "linux", "nginx", "ssl"]
+++

In [Episode 11](https://www.unusually.pink/podcast/episode-11-attach-all-the-storage), I had discussed how I run a couple of my websites on a Linux server running [Nginx](https://www.nginx.com/) as the web server and encrypting connections to them via [Let’s Encrypt](https://letsencrypt.org/) certificates. Shortly after recording that episode, though, I realized I had messed up my certificate configuration via [certbot](https://certbot.eff.org/). If you don’t recall the episode, I had taken my web server which was only running [laifu.moe](https://laifu.moe) and added [awk.ninja](https://awk.ninja) to it so that I had both sites running on the same server. When I added awk.ninja, I had to re-run certbot and get a certificate for it along with the certificate I had for laifu.moe. That’s where I messed up; I got tipped off when I received the following email from Let’s Encrypt letting me know that my certificate for laifu.moe was about to expire.

> “Your certificate (or certificates) for the names listed below will expire in 10 days (on 07 Jul 19 12:52 +0000). Please make sure to renew your certificate before then, or visitors to your website will encounter errors.
> 
> We recommend renewing certificates automatically when they have a third of their  
> total lifetime left. For Let’s Encrypt’s current 90-day certificates, that means  
> renewing 30 days before expiration. See  
> https://letsencrypt.org/docs/integration-guide/ for details.
> 
> laifu.moe  
> www.laifu.moe”

That seemed odd to me since I knew I had a **cron** job running to update the certificates. I checked the expiration for the certificate on laifu.moe and saw that it had nearly two months left on it. I checked the certificate applied to awk.ninja and saw the same thing. EXACTLY the same thing in fact. In double-checking the certificate on laifu.moe, I realized that the Common Name was for awk.ninja. I was using the awk.ninja certificate for _both_ of my sites. Oops. What happened was that when I added awk.ninja and re-ran **certbot**, I got the following:

![](/images/FixingLetsEncryptCertificatesAfterYouDeleteThemLikeAnIdiot_certbot.png)

My thought at the time was that I needed to select ALL of the sites. In reality, this overwrote the configuration I already had on laifu.moe and applied the awk.ninja certificate to both sites. This is where I decided to be really stupid. I decided that I would delete the existing certificates, re-run **certbot** twice (one for laifu.moe and once for awk.ninja), and then be done. I started off by deleting the awk.ninja certificate that was applied to both sites:

```shell
sudo certbot delete --cert-name awk.ninja
```

I did the same to delete the laifu.moe certificate. Then I tried to do a vanilla run of **certbot** to get the menu in my screenshot above and individually configure each of my two sites. Instead of getting that menu, though, I received an error message that my sites were pointing to certificates that didn’t exist. **certbot** then exited without giving me any further options. The problem is that my configuration files below still referenced the certificates that I just nuked. Oops.

```
/etc/nginx/sites-available/awk.ninja
/etc/nginx/sites-available/laifu.moe
```

After thinking about it for a few seconds, it made sense; **certbot** can’t know what’s going on and is expecting me to do some cleanup on the mess I made instead of making assumptions about whether or not I should still have certificates. To keep my life simple, I decided to go back to a clean slate on my **sites-available** configurations since I knew that I could get **certbot** to redo the configuration again as long as I could get it to successfully run. As a result, I just set the configurations for both laifu.moe and awk.ninja back to a super vanilla setup. Just **%s/laifu.moe/awk.ninja/g** on the file below for what I configured on awk.ninja.

```
server {
        listen 80;
        listen [::]:80;

        root /var/www/laifu.moe/html;
        index index.html index.htm index.nginx-debian.html;

        server_name laifu.moe www.laifu.moe;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

Once I had that done, I restarted nginx just to make sure it was working and I could hit port 80 for both sites.

```shell
sudo systemctl restart nginx
```

With that working, I was able to re-run **certbot** and finally get the menu from my initial screenshot. I first configured a certificate for awk.ninja and its www variant. Once that was done, I ran **certbot** one more time and walked through getting a certificate for laifu.moe and its www-variant. In both instances, I opted to have **certbot** reconfigure the files in **sites-available** to redirect all HTTP traffic for HTTPS. I restarted Nginx one more time and _finally_ I had everything configured the way I wanted with each site using its _own_ certificate.

The moral of the story is to actually troubleshoot the problem instead of just starting off by deleting shit from your server. Also, try staying pink!
