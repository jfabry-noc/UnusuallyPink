+++
title = "Unusually Pink Updates: Dropbox, Password Managers, and Ad Blocking"
date = "2019-06-06"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["1password", "ad-blocking", "chrome", "chromium", "dashland", "dropbox", "firefox", "keepass", "password-manager", "ublock-origin"]
tags = ["1password", "ad-blocking", "chrome", "chromium", "dashland", "dropbox", "firefox", "keepass", "password-manager", "ublock-origin"]
+++

This will be one of those weird amalgam posts featuring multiple only mildly-related topics but which are too short to be a post in their own right. I though it would be helpful to provide some updates on a few things that Brandi and I have talked about in some of our [past episodes](https://www.unusually.pink/podcast). They’re all tech-related… but you already know that from reading the title.

## Dropbox Upgrades and Price Increases

On June 1st, Dropbox added more features to their [Plus plan](https://www.dropbox.com/individual/plans-comparison). I’ve been a Plus customer for a couple of years now. It’s less of a “I need to pay for the extra space” situation and more of a “I store some important shit in here and this gives me more peace of mind” situation. The main features from the upgrade, according to the email they sent me, are:

> “Double your storage—save everything with 2 TB (2,000 GB).  
> World-class sync technology—move out-of-date files off your computer’s hard drive and to the cloud with Dropbox Smart Sync.  
> Dropbox Rewind—roll back accidental changes to any folder, or your entire account, up to 30 days.”

The extra storage is nice, though suffice to say it’s not exactly something I **need**.

![](images/UnusuallyPinkUpdatesDropboxPasswordManagersandAdBlocking_dropbox_space.png)

Rewind seems **really** nice, though. I store a lot of PowerShell scripts in Dropbox, so I could see rewind being useful if I accidentally break one or need to snag an older version for some reason. While I’ve recently been making an effort to check in my bigger, more important scripts with [Azure DevOps](https://azure.microsoft.com/en-us/services/devops/) for version control, smaller ones that I’m the only person likely to use still just go into Dropbox.

Things aren’t all roses and unicorns, though, as the added storage and features come at a cost: $2 USD more per month. This bumps the monthly price up to $12 USD. The good part is that if you go with yearly pricing, you save that $2 a month and pay $120 for the year. I’m actually still on monthly pricing despite having used the Plus service for a few years so I’ll need to move over for sure. While the bonus storage is available now the new pricing isn’t happening for me until July. So before early July I need to swap to yearly billing to avoid paying the extra couple of bucks.

## Wired Password Manager Recommendations

In our [last episode](https://www.unusually.pink/podcast/episode-8-playlists-and-privacy), Brandi and I spoke a bit about password managers. Right after we recorded that episode [Wired published an article all about password managers](https://www.wired.com/story/best-password-managers/). The timing seemed perfect to share it. Of the ones Wired recommends Dashlane is the only that I haven’t used. It also makes me feel good that my preferred password manager, [1Password](https://1password.com/), was Wired’s #1 recommendation.

Speaking of Wired, I love their content but always run into my 5 free articles per month limit. I just happened to see earlier today that they’re [doing a deal](https://subscribe.wired.com/subscribe/wired/121031?source=EDT_WIR_TWITTER_0_WEEKOFJUNE03_3_ZZ&utm_brand=wired&utm_source=twitter&utm_social-type=owned&mbid=social_twitter&utm_medium=social&utm_campaign=wired) where you can snag a year’s subscription for just $10. This includes the print editions plus digital content (normally $50 a year) or just digital if you don’t care about print (normally $30 a year.) I was fortunate enough to see it right when [they posted to Twitter](https://twitter.com/WIRED/status/1136709547022848006). I have no clue how long this is running for, but it seems pretty worthwhile if you’re a geek and like tech news. And no… we don’t get anything for promoting this. I just like Wired.

## Browser Ad Blocking

Also from our [last episode](https://www.unusually.pink/podcast/episode-8-playlists-and-privacy), we discussed how [Google was initially thinking about making changes to the webRequest API](https://www.zdnet.com/article/google-chrome-could-soon-kill-off-most-ad-blocker-extensions/) that would essentially cripple ad blockers. Like we discussed in the episode, after those initial reports and backlash in January Google had backed off a little and said they would think through how this API update should work. At the end of May, though, it came to light that [Google still plans to nuke the webRequest API pieces that allow current ad blockers to work](https://www.androidpolice.com/2019/05/29/google-still-plans-to-kill-chromes-existing-adblock-apis/)… that is [unless you’re a G Suite customer](https://9to5google.com/2019/05/29/chrome-ad-blocking-enterprise-manifest-v3/).

What’s worse is that these proposed changes seem to be in the [open-source Chromium project](https://www.chromium.org/). So unless Microsoft does some work on their fork, the [Chromium-based version of Edge](https://www.microsoftedgeinsider.com/en-us/) could also be impacted. Suffice to say this gives me some **significant** pause on my previously mentioned plans to buy a new Chromebook. I’m now re-evaluating what I should do for my laptop situation. And if you haven’t used [Firefox Quantum](https://www.mozilla.org/en-US/firefox/) recently, now might be a great time to check it out.
