+++
title = "Connecting Irssi To Twitch.tv IRC"
date = "2020-03-29"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["chat", "irc", "irssi", "twitch", "twitchtv"]
tags = ["chat", "irc", "irssi", "twitch", "twitchtv"]
+++

![](images/ConnectingIrssiToTwitchtvIRC_twitch_irc_photo.jpg)

A couple of days ago Brandi decided to try streaming her latest obsession, _Animal Crossing: New Horizons_ on [Twitch](https://twitch.tv). As the unofficial game of the coronavirus quarantine, it seemed like a good idea. When she told me, I decided to do the comfortable thing and turn on her stream from the Twitch app on my Amazon Fire TV Stick so I could watch it on my TV. This works well for most of the streams I might happen to watch because I never actually care about interacting with the chat. While the app on my Fire TV Stick does allow me to view the chat, trying to type anything via a remote's D-pad and an on-screen keyboard on my TV would be painful to say the least.

In this particular instance, though, I wanted to be able to chat with Brandi and a few other friends who popped into her stream. I initially had her stream open on my phone and was chatting there, but it was annoying to have to keep waking my phone up to look at the chat. Then I opened her stream in a browser on my [Pinebook Pro](https://www.unusually.pink/blog/unusually-pink-impressions-pinebook-pro). I simply paused the stream since I didn't need it running twice, but this left a tiny sidebar for the chat that was less than ideal.

Being a regular user of IRC, I was aware that Twitch chat is just an IRC channel under the hood; this is pretty obvious given that the bots most decent-sized Twitch streamers employ in their chats are just IRC bots. I'd know since I once wrote a really shitty IRC bot for an IRC server I ran for my team at a previous job. As such, I figured I could just connect a regular IRC client to Twitch's IRC servers, find Brandi's channel, and chat that way. Given that I [already use IRC regularly](https://jfabhd.com), I have a server that I normally connect from via my favorite client, [Irssi](https://irssi.org).

Getting the connection right took a little doing as finding up-to-date information on Twitch's IRC setup is a bit spotty. I ended up combining information from a bunch of different sites before I got everything to work properly.

I'm not going to cover how to get and access Irssi; if you need help with that you can use [their own documentation](https://irssi.org/documentation/). Assuming you've got access to Irssi and you've just launched the application, the first thing to do is to define a new network for Twitch; for any of the below examples just replace what appears in \[brackets\] with your own information:

```
/network add -nick \[username\] Twitch
```

This defines a new IRC network in Irssi and states what your nickname will be for it, which is just your Twitch.tv username. You could technically name the network anything you like, you don't have to call it "Twitch", but simplicity is generally the best approach. The next step is to define the server information for that network.

```
/server add -auto -ssl -network Twitch irc.twitch.tv 6697 \[oauth password\]
```

This defines the server of `irc.twitch.tv` for the Twitch IRC network, specifies the port to use, and gives the password to authenticate your account. **Note:** Your OAuth password is **not** the same as your regular password to log in to Twitch! You must set that up at the [OAuth password setup page](http://twitchapps.com/tmi/) for Twitch. Then copy and paste the password value you've generated into the command above. You _must_ leave it prefaced with "oauth:" just like how Twitch provides it to you in the command; don't strip that off the front.

With all of that done, now you can simply connect to your newly defined IRC network via:

```
/connect Twitch
```

This should show you a message letting you know that you've successfully connected. The channel to join is simply the name of the Twitch account prefaced by a hashtag just like every other IRC channel. So to join a particular channel you would just type:

```
/join #favoriteStreamerName
```

That's all there is to it! You can now happily chat with a stream from Irssi while watching the actual video feed on another device. I assume the steps above could be pretty easily translated into a different IRC client if you prefer something other than Irssi.

![](images/ConnectingIrssiToTwitchtvIRC_irssi.png)

There are, however, just a couple of caveats to keep in mind. As you might imagine, there is no support for Twitch emotes within IRC. When someone else in the chat uses an emote, for example, you will simply see the plaintext rendition of it. So if someone uses the BibleThump emote, you will see that text rather than the character from [The Binding of Isaac](https://bindingofisaac.com).

The other main caveat is that as a guest in the channel you cannot see the other participants of the chat from IRC if you check who is in the channel. You can, however, still see whatever messages they send, and Irssi will still auto-complete their names if you want to mention them. For example, when running Irssi's `/names` command to see a list of channel members, I see only myself:

![](images/ConnectingIrssiToTwitchtvIRC_irssi_names.png)

As you can see in the screenshot above, a friend of ours is also actively using Twitch chat, but I am the only member listed in the channel. This isn't a big ordeal once you realize it (unless you're really jonesing to see everyone's name for some reason), but it did trip me up initially since I had assumed that I was not connecting to the proper channel. I eventually left both IRC and Twitch chat open in a browser until someone else typed something so that I could verify the same message appeared in both places.

On the whole I found this to be a streamlined, elegant way to actually participate in Brandi's Twitch chat from my low-end laptop while watching her actual video stream on my TV from the comfort of my couch. Plus, you just get to feel awesome for using IRC.
