+++
title = "The Dumpster Fire Of Remote Podcasting"
date = "2020-04-19"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["anchor", "podcast", "podcasting", "remote", "ringr", "squadcast", "zencastr"]
tags = ["anchor", "podcast", "podcasting", "remote", "ringr", "squadcast", "zencastr"]
+++

As we mentioned in our [last episode](https://www.unusually.pink/podcast/episode-20-quarantine-with-animal-crossing), we've switched to recording our podcast episodes remotely due to the fact that there's currently a global pandemic happening. It's been a big change considering that every other episode of both [this podcast](https://unusually.pink/podcast) and the [Same Shade of Difference](https://sameshadeofdifference.com) podcast have been recorded in person.

Switching to a remote recording setup has been trying since, to be completely honest, most of the services for remote podcasting are _horrible_. They might be good services if you want to do some pretty hefty editing and audio engineering work; if you want just a means to do a recording and get a blended .wav or .mp3 file, though, things are surprisingly difficult.

## SquadCast

The first service we tried was actually one that I tested with [Craft Brew Geek](https://instagram.com/craftbrewgeek) for a recording of the [Same Shade of Difference](https://sameshadeofdifference.com) podcast. We gave [SquadCast](https://www.squadcast.fm) a try since we actually went to their booth at [Podfest](https://www.unusually.pink/blog/podfest-multimedia-expo). The big claim to face for SquadCast is that it provides you with both audio **and** video. At the time of this writing it won't record the video feed, but it honestly can't be overstated how beneficial it is to see the people you're recording with. Being able to see body language and facial expressions does a _lot_ for helping the conversation proceed more smoothly and to prevent everyone from accidentally talking over one another.

Unfortunately, the execution of SquadCast left quite a bit to be desired. The first issue we had with SquadCast is that, when everything is said and done, it provides a .wav file for _each participant_. You have to piece the files together on your own, which is not the simple, streamlined experience we had been hoping for. It gets worse, though, because we discovered that any time someone switches their audio input (e.g. if you have a less technically inclined guest who keeps having connectivity problems with their Bluetooth microphone), it completely stops the recording for **everyone**. So we ended up having 3 .wav files for each of the 4 participants of our recording session. That's 12 .wav files of all variable lengths that we would've needed to painstakingly put together.

If having that many files feels ridiculous, apparently whoever does the frontend development for SquadCast agrees; their Dashboard that gives access to the recordings wouldn't even properly render all 12 files for Mark to download them. He actually had to use his browser's function to zoom out so that the UI would properly render access to all of the files. Ick!

## Zencastr

After such a terrible experience with SquadCast, Brandi and I decided we weren't even going to bother with attempting to use it for the Unusually Pink Podcast. Instead, we decided to give [Zencastr](https://zencastr.com) a shot. Zencastr is a fairly popular option, and it's also currently making its service completely free while everyone is stuck at home due to COVID-19. Big kudos to them for that!

Unlike SquadCast, Zencastr is focused on the audio only; you don't get a video feed. However, what you **do** get is the same, janky mess of individual .wav files for each participant. To not repeat the mistakes Mark and I made, Brandi and I just attempted a couple of quick, test-drive recordings with Zencastr so we didn't record an entire episode that we ended up scrapping a la SquadCast. I'm 100% willing to own responsibility for doing something incorrectly, but Brandi and I also ended up with multiple files from Zencastr. Not only were there individual .wav files for each of us, but Brandi's recording randomly had two .wav files, and the duration of her two files combined was still less than the duration of my single .wav file.

Once again, we may have been able to splice them together, but that's not the workflow we want; we've been spoiled by having the RODECaster Pro spit out a single, combined .wav file that we can do any edits on.

## Anchor

I've seen lots of chatter about [Anchor](https://anchor.fm) being the most drop-dead simple option for podcast recording, so I figured I'd spend a little time digging into its options. Unfortunately, it seems to fall into the same [mobile-first trap as Google WiFi](https://www.unusually.pink/blog/google-wifi-and-the-curse-of-simplication) where all other functionality is severely limited. For example, operating out of the web app for Anchor only gives you the option to record a solo episode; you can't seem to loop in any other audio feeds.

The Anchor mobile app, however, does allow you to record with multiple people at the same time. We _strongly_ wanted to avoid operating from mobile, though, because both Brandi and I have at least decent-ish USB microphones we wanted to use without trying to go through dongle-hell to connect them to a phone or tablet.

Anchor's other oddity is that it seems to operate under the assumption that it will be your podcast hosting platform. We would essentially need to take the recording for our episode and have it published to Anchor; only instead of the Anchor RSS feed being dispersed to all of the big podcast platforms, we'd then need to download a local copy of the recording from Anchor in order to edit and re-upload to our actual host.

## Ringr

The last service Brandi and I attempted was [Ringr](https://www.ringr.com). To be honest, the service seemed more than a little sketch initially. The web app was listed as being a "beta" with the more established option being the mobile apps. Brandi and I quickly glanced at their mobile apps in our respective app stores only to discover that neither the iOS or Android apps had been updated in over a year. Regardless, we did a quick test with the Ringr desktop app and, amazingly, had decent luck (other than a minor, non-Ringr specific caveat that I'll mention at the end.) The two of us were able to do a recording in Ringr and get a plethora of recording options after the fact. We could simply download an .mp3 file that had both of our audio feeds combined together. If we wanted a lossless option and had something to handle the format (GarageBand won't), we could also download a FLAC file with the audio from each of us combined. Or, if we actually wanted what the other services offered, we could download raw .wav files with an individual file for each input.

Suffice to say, we were happy enough to snag the .mp3 that had both audio feeds spliced together and move on. Ringr seems to operate by recording at each end, and then once the recording is stopped the web app will upload the audio from each endpoint (you have to wait for it to do this prior to closing the browser window for your recording session) and then splice it together. Additionally, it seems to do some post-processing on the audio files. For example, during our initial recording tests Brandi noted that she could actually hear white noise from my end due to my microphone picking up the sound of my refridgerator kicking on. In the finished .mp3, though, the white noise was nowhere to be found.

Ringr is far from perfect, though, Like most options, it doesn't provide any video. Brandi and I had to be careful to avoid speaking over one another... and we still struggled with it. While our first episode recorded with Ringr seemed fine, in the second episode Brandi was randomly disconnected from the call. While Ringr gives you options to reconnect to a disconnected call, it never actually **works**. The few times we ran into this scenario, Brandi and I would end up in separate instances of the same call, without any ability to hear one another. So we'd need to schedule a brand new call and start over again. In the instance where she got disconnected near the end of an episode, I just tried to do the outro on my own once I realized what was happening and we were able to salvage the situation. It's extremely janky, though, and would've been much more problematic if it happened around the halfway point in our recording session instead of at the end.

While that's a significant problem, Ringr has still been the best option for us by far. If you're struggling to record your podcast remotely during quarantine, our recommendation would be to check out Ringr for the easiest experience.

## macOS And Firefox Caveat

We did run into a caveat for any web-based services; as of the time of this writing there seems to be an issue when using a Blue Snowball USB microphone as your audio input with Firefox in macOS. When I initially tried using SquadCast with Mark, my microphone sounded like absolute trash; I sounded like a robot transmitting my audio over an extremely static-filled connection from an OG Soviet submarine. After switching from Firefox to Chrome, though, the issues went away. Brandi saw the same behavior when joining Ringr calls through Firefox. As soon as I asked her to switch to a different browser, though, everything was fine. While I personally try to avoid using Chrome whenever possible in favor of Firefox or Safari, in this instance the safest bet is to use Chrome for any recording just because it seems to be the best supported option.
