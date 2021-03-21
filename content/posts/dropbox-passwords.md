+++
title = "Dropbox Passwords"
date = "2020-09-09"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["dropbox", "dropbox-passwords"]
tags = ["dropbox", "dropbox-passwords"]
+++

I tend to pay for a _lot_ of subscription services. In fact, my friend Mark and I have enough of them between us that we needed not [one](https://sameshadeofdifference.com/episodes/mo-money-mo-money) but [two](https://sameshadeofdifference.com/episodes/even-mo-money) episodes of our podcast just to talk about all of our subscriptions. Since the pandemic means I have nothing better to do with my time than sit around and think about things like how much money I spend on subscriptions, though, I've been thinking about which ones I might be able to do without, which ones I could swap for cheaper services, etc. to save myself a little bit of money each year. It often feels trivial to tack on yet another thing that costs $5 - $10 a month, but over the course of the year it adds up.

Enter [Dropbox Passwords](https://www.dropbox.com/features/security/passwords), a password manager built into Dropbox. In the past I've used Dropbox to sync passwords in conjunction with [KeePassX](https://www.keepassx.org/), so having the same functionality built directly into the platform seemed nice. The fact that it's a feature included with my Dropbox Plus plan and would save me from paying $80 a year for my current password manager is also a nice bonus. First I just had to put it through its paces.

## Migration

Migrating to a new password manager is typically a fairly painless process. Every password manager I've ever used has given me the option to export my passwords to a variety of plaintext file formats. Naturally, having a plaintext file with all of your credentials is a _terrible_ idea, but unless the machine you're operating on is a digital cesspool it should be fine for the few minutes it takes to import the file somewhere else.

In my case, I exported a CSV and imported it into Dropbox Passwords. I initially got a message that there weren't any accounts to import. I opened the CSV file and saw that some of the columns had weird headings and assumed Dropbox didn't know what fields in the CSV mapped to which fields within Passwords. Their [help documentation](https://help.dropbox.com/installs-integrations/desktop/how-to-use-dropbox-passwords) covers what's needed:

> The columns in your CSV file must be labeled so Dropbox Passwords knows how to import the information. Although Dropbox Passwords can recognize a range of labels, we recommend labeling them “Name”, “Password”, “Username”, “Notes”, and “URL”.

I updated the column headings to match the documentation above, and everything was fine.

## Desktop Client

The desktop client for Dropbox Passwords is spartan to say the least. You get fields for:

- Site Name
- Username
- Password
- URL
- Notes

That's it. In other password managers, I frequently leverage either additional passwords or custom fields to add things like app passwords, API keys, etc. While I could store those in the free-form Notes field in Dropbox Passwords, the values aren't masked out like they would be in other services with dedicated fields for this sort of thing.

After the initial setup where I logged in with my Dropbox credentials, the app gave me a "word list." This PDF just had 12, random, English words on it. This serves as an extra security mechanism that I'll touch on in the next section.

After the app was set up, it asked me to create a 6 digit PIN. That PIN is used to unlock the app if it times out due to inactivity. It's worth noting that the browser extensions will _not_ autofill login information is the application is currently locked; more on that later as well.

## Mobile Apps

There isn't too much to say about the mobile apps; they're basically exactly what you would expect. It is worth mentioning that, at the time of this writing, there's no iPad version of the app, meaning I'm stuck looking at the blown-up iOS app. It's not a huge ordeal, though, because aside from logging in initially I almost _never_ open the app itself. Like every other password manager, iOS can be configured to automatically get passwords out of it without requiring an app switch. It also integrates with Face ID and Touch ID on iOS for quick unlocking.

## Multi Factor Authentication

Dropbox Passwords automatically implements a sort of MFA. When I logged in to the app on my phone, for example, it gave me a prompt on the desktop client. I had to accept the prompt there to confirm that I was, in fact, trying to configure the app on a phone. Likewise, when I configured the app on my iPad, I received a prompt on both my laptop and my phone.

This is where you might wonder what happens if I don't have any of those other devices handy. In that case, I can use the word list to log in. I actually ended up doing this one time, and it worked without a hitch. What happens if I also lose the word list? Let's hope I never find out. It's nice to know, though, that despite the fact that the content is tied to a Dropbox account, Dropbox account credentials alone aren't enough to access it.

## Browser Extensions

You might wonder why I talked about desktop and mobile clients, switched gears to authentication, and then came back to a "client." The reason is that the browser extensions are literally just a wrapper that provides integration with the desktop app for things like autofilling credentials. For example, clicking on the Dropbox Passwords extension icon in Safari on macOS doesn't even open a UI for the extension... it pops open the full Dropbox Passwords client. I see this frequently when nothing autofills in my browser, I click on the icon for the browser extension, and then it opens the full app where I see it requesting my PIN to unlock it.

The reason why wrapper browser extensions are noteworthy for me is that there are **no** standalone extensions or even direct web access. If Dropbox Passwords doesn't have a client on your platform of choice, you're simply out of luck. For example, I can't access my passwords when using Manjaro Linux on my Pinebook Pro. I verified this by installing the browser extension; clicking on it will bring me to a lovely message that the application isn't available for my platform.

![](images/dbp_linux-1024x464.png)

Where this seems _really_ insane to me is that if I log into my Dropbox account on the web I can _see_ the vault for Dropbox Passwords! But clicking on it gives me the same screen as shown in the image above.

![](images/dropbox.png)

I can't actually do anything to access it. Even _just_ some kind of web portal like I can access with [Bitwarden](https://bitwarden.com/), [LastPass](https://www.lastpass.com/), or [1Password](https://1password.com/) would be better than **nothing**. I can definitely understand not making a native Linux app a priority, but not having a browser extension or web access in 2020 blows my mind more than a little.

I **really** hope this is something the Dropbox Passwords team is actively working on. While the overall service isn't quite as slick or polished as some of its competitors, the fact that it comes included with paid Dropbox Plans is a huge boon; people like myself will have to think twice about paying extra money for a service they already have included with their existing Dropbox subscription. There are some hurdles to overcome for Dropbox Passwords to reach parity with its competitors, but for many people it'll be good enough as-is.
