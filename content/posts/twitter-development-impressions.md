+++
title = "Twitter Development Impressions"
date = "2020-08-10"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["scripting", "twitter", "rest", "api"]
tags = ["scripting", "twitter", "rest", "api"]
+++

I recently had an idea to turn the Twitter account for my defunct podcast into a Twitter bot posting a new shade of pink every day; it makes sense because the podcast and Twitter account were centered around the color pink. I didn’t really think anyone would care about this particular bot, but it seemed like a fun project idea to work on. It ended up being an interesting learning experience, but not at all for the reasons I actually expected.

## Developer Account

Getting a Twitter developer account can be either really simple or really irritating, and there’s no discernable difference that dictates what experience you’ll get. I went to the [Developer portal](https://developer.twitter.com/en) and registered for a developer account with my normal Twitter account. This account is clearly me IRL; my name is in it, I have a photo of myself on the account, it links to my personal website, and the post history clearly indicates that the account is a person rather than a bot. As part of registering for the account, I had to describe what I was going to create. I honestly stated that I was just going to create a bot that would tweet a shade of pink each day. Twitter asks questions such as if you plan to export data out of the service, if you plan to display information posted to Twitter outside of Twitter, etc. I answered “no” to all of these questions since I wasn’t pulling any data out of the service. I just needed to post.

After I completed the registration form, I received a message that my account was under review and I would receive a notification when that review was completed. I was a little bummed since it happened to be a long weekend for me, and I was hoping that this project would give me something to fill the time. I was hopeful maybe the review would be completed quickly. I was wrong. It took just shy of 2 weeks before the review was completed. I had almost forgotten about the whole thing since I’ve been trying to stay off of Twitter as of late, but then I got an email telling me I was allowed in. Wild.

## Authentication

Handling [authentication with Mastodon](https://docs.joinmastodon.org/methods/apps/oauth/) is a relatively simple, straightforward process if you’ve done this sort of thing with… pretty much any API. It’s a little different than the type of things I do for work since creating a client means people _other_ than the person writing the code can be authenticating, but it still makes sense and is well documented. On the other hand, [authenticating through Twitter](https://developer.twitter.com/en/docs/basics/authentication/oauth-1-0a) is a complete nightmare. Outside of the specifics of authentication, everything in Twitter’s documentation seems aimed at keeping each individual page as short as possible. As a result, every page links to numerous other pages, and you end up having dozens of browser tabs open just to have some clue as to what your complete workflow looks like. For the OAuth 1.0a option, which is _the_ option to use if you need an account other than the registered developer account to leverage an application, they recommend strongly against making the [JWT](https://awk.ninja/posts/groovy_jwt/) yourself in favor of leveraging a library… but they don’t actually share any of the particulars about their JWT setup… or even _call_ it a JWT. You very clearly get the impression that Twitter doesn’t want anyone actually using their API. Crazy.

After seeing the poor documentation, I abandoned my ideas of making my bot in Rust or maybe Bash, and instead just decided to use Python with the [twython library](https://github.com/ryanmcgrath/twython). I’ve manually parsed together enough JWTs that I didn’t think I cared enough to do it again for this. Seeing the workflow for twython showcased the next bit of crazy, though, which is that the [authentication workflow sends the OAuth token to the callback URL](https://developer.twitter.com/en/docs/basics/authentication/oauth-1-0a/obtaining-user-access-tokens). I basically needed to set up something completely different with an HTTP listener for the OAuth token so that I could move forwad with authentication. That was _entirely_ more than I wanted to put into this simple bot.

**Note:** _The craziness of this makes me still think I’m not actually understanding the setup properly. I did verify, though, that the response received from where I was running the code included just the HTTP status, so the information is not coming back to the sender by default. Likewise, I couldn’t open my application up to other users without giving a callback URL, so omitting that wasn’t an option, either. Hit me up on Mastodon if I’m just dumb and there’s a reasonable way to handle this that I’m misunderstanding._

## Registering Another Developer Account

At this point I realized that I really should have just registered for a developer account with the account I was planning to use with the bot. I started that process again fully expecting to wait another two weeks. I filled out all of the same information during the registration process, but this time when I completed the form I was immediately kicked over to the developer portal to start working on whatever I needed.

I’m still amazed that while the registration for my actual account that I use as a human being, I had to wait two weeks to get developer access. When I registered witht the account that had been used for my podcast, though, I was allowed access sans review. The podcast Twitter account generally had nothing posted to it other than the automatic posts from [Squarespace](https://www.squarespace.com/), had the podcast logo as the profile picture, had no website linked, and quite clearly was not a person… yet that’s the account that got in right away. Okie dokie!

## Wrap-Up

Since I now had the account I was planning to post from in the developer portal, I could spin up my OAuth token directly from there in my browser as opposed to having to leverage 3-legged OAuth to a callback URL. As I started working on the code, though, I quickly realized that the script for the bot to post was going to be insanely easy. Instead, the much more interesting part of the code was the script I wrote to make a little local repository of information on shades of pink that was completely unrelated to Twitter. I had originally planned to cover that in this post, but since this ended up being longer than I expected that’s what I’ll cover next time around.
