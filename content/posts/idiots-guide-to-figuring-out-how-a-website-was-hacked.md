+++
title = "Idiot's Guide To Figuring Out How A Website Was Hacked"
date = "2020-03-18"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["dfir", "infosec", "security", "wordpress"]
tags = ["dfir", "infosec", "security", "wordpress"]
+++

**_Full Disclosure:_** _This won't tell you exactly what was wrong with a website. This will just give you a pretty good, quick idea. I'm not in DFIR or even InfoSec. I'm just a sysadmin who has some familiarity with a decent number of systems. It's also worth mentioning that I did all of these actions from my Linux machine. The same would be possible from macOS or from Windows 10 with the Windows Subsystem for Linux._

Last night, my good buddy [Craft Brew Geek](https://www.instagram.com/craftbrewgeek/) shot me a message because a website we both had something of an interest in (I won't go into more specifics than that to protect the guilty... I'll just say that it doesn't belong to either of us) had suddenly exhibited weird behavior. Navigating to the website, either via directly typing their URL into my browser or by searching for them on Google and clicking the link, took me _not_ to the expected website but to a **super** shady online pharmacy; there's not enough booze in the world to get me drunk enough to type my credit card information into this site. Since we're all stuck at home under quarantine, though, I figured I'd kill a little time digging into what, exactly, was going on.

The initial problem is that I navigate to `desiredsite.com` and it takes me to `shadysite.com` instead. A common way this type of thing happens without any degree of technical compromise is if someone allows their domain to expire rather than having it automatically renew. When that happens, it's possible for an attacker to swoop in, buy the domain, and then change the DNS information to point to their desired site. It's pretty uncommon since most DNS registrars will park domains for a month, giving the original owner time to renew. Failing that, they often go to auction rather than back into the pool. Additionally, under this scenario there would be no reason to redirect to `shadysite.com`. Still, it doesn't hurt to check the DNS history through something like [SecurityTrails](https://securitytrails.com/). This showed the last DNS change was 3 months ago for the site in question; there's no way the site had been redirected for 3 months so I could rule that out.

My next thought was to see if the sites were on the same server. If they were, that would tell me the entire server was wrecked, receiving my request, and was configured to load a different site instead. This was easy enough through `dig`:

```shell
dig +short desiredsite.com. a
dig +short shadysite.com. a
```

This gave me two different IP addresses. This tells me the sites aren't hosted on the same server, which means that `desiredsite.com` is _redirecting_ me to `shadysite.com`. For that to be the case, I _have_ to be hitting `desiredsite.com` first, but then I'm redirected before I see anything. I needed to see what was up with the site before being redirected. Scripts on the web are most commonly executed not on the server side, but locally in the browser. As a result, I used `wget` to just try to snag the file living at `desiredsite.com`, which for most websites will be `index.html`:

```shell
wget http://desiredsite.com
```

This simply downloads the file to my local machine. Nothing is actually executing any scripts it might reference. Sure enough, this gives me an HTML file for `desiredsite.com` I can open in a text editor. I figured JavaScript was likely being used to handle the redirect. To test this, I turned off JavaScript in Google Chrome and once again navigated to `http://desiredsite.com`. This caused the expected website to load, albeit kind of broken since JavaScript wasn't running.

Diving back into the `index.html` file, a quick search showed me that there were nearly 60 `.js` files for JavaScript. Ick. JavaScript can be written to be fairly easy to consume if you've got a passing familiarity with computer programming, but _most_ JavaScript on the web is designed to be 1.) minified and 2.) obfuscated to make this nearly impossible. Seriously, this is what a typical JavaScript file looks like. Note how my editor is showcasing the fact that it's all **one line**:

![](images/IdiotsGuideToFiguringOutHowAWebsiteWasHacked_gross.png)

Clearly trying to read through 60 files of that isn't going to happen; this isn't my job, I'm doing it for fun. However, I still had some options for trying to quickly look for something flagrant. I saved down local copies of all 60 JavaScript files in the same directory, and then navigated to that directory from my terminal. I then used `grep -R` to recursively search through every JavaScript file at the same time.

```shell
cd /path/to/javascript
grep -R "search term here" .
```

What did I search for? I started off by searching for `shadysite.com`. No dice. Then I searched for the IP address I got for the site from my previous `dig` command. Also no dice. I didn't think it would be anything that overt, but it was worth a shot. I decided to look at the source code for `shadysite.com` to see if there were any clues. I immediately noticed that the entire site was coded around the IP address for the site rather than the domain. For example, links in the source code of most sites are going to look like:

http://mydomain.com/folder/page.html

The links on this particular page were done like this:

http://192.168.254.254/folder/page.html

Obviously that _wasn't_ the IP address in use, but you get the point. This tells me that, unsurprisingly, they run into a lot of problems with their domain getting shut down. So they design the site to be domain agnostic, buy a new domain when the old one is shuttered, and then point it to the same IP address they've been using. Some quick searches online showed me a few tools I could use to plug in an IP address and get a historical list of domains tied to that IP. I used [ViewDNS.info](https://viewdns.info/reverseip/). This showed me 6 total domains that had been pointing to the same IIP address, one of which was what I saw now. I repeated my `grep` search above with the others to see if there were any hits, but sadly there was still no luck.

At this point, though, I still had a pretty good idea of what was happening. Out of the 60 JavaScript files referenced by the source code for `desiredsite.com`, most of them were in a sub-directory for WordPress, including some directories that noted they were for WordPress plugins. Having looked at enough compromised websites over the past 15 years, it's a definite trend that WordPress (and especially WordPress plugins) tend to be Swiss cheese. WordPress plugins are a frequent target for attackers, and most people _never_ think to update them. At this point, if I were determined to get to the bottom of things, it would be much quicker to just point some kind of vulnerability scanner like [Nessus](https://www.tenable.com/products/nessus/nessus-professional) at the site and just let it find the vulnerable plugin(s) rather than tracking them down through obfuscated JavaScript.

All told, though, it was a fun exercise to dig into how the site was compromised and come away after only about 30 minutes of work with a pretty good guess.
