+++
title = "Parse ISO 8601 String With Timezone Offset To Date Object In Groovy"
date = "2021-01-18"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["date", "datetime", "groovy", "groovy-programming", "iso-8601", "programming", "scripting"]
tags = ["date", "datetime", "groovy", "groovy-programming", "iso-8601", "programming", "scripting"]
+++

My [Groovy posts always have the absolute worst titles](https://unusually.pink/groovy-programming-creating-an-iso-date-at-a-specific-date-and-time-in-utc/), but I figure that making them verbose is really the only way to make them discoverable. I write a good bit of Groovy code at work, and given that it's been a few years since the heyday of Groovy it can occasionally be a bit of a struggle to figure out exactly what I need with the resources I dig up online. Any time I manage to figure out something that was difficult to find information on, it seems like a good opportunity to fill the gap a bit.

I do a lot of work with APIs, mostly making calls against cloud-based systems. In the cloud, no one generally cares about what timezone you happen to be in, so UTC is always used. I've recently been making calls against local software platform, though, and **it** returns timestamps in the local time of the server running it. Ick. I ended up with string values like the following which I then needed to parse as a Date object in Groovy for the sake of comparison.

```
"2020-10-17T02:00:21.644-04:00"
```

I'm quite familiar with converting something like this _without_ the timezone offset at the end; that one was new for me. My first struggle was exactly what to search for in order to dig up information on this; I eventually discovered that this is [an ISO 8601 date and time format](https://www.iso.org/iso-8601-date-and-time-format.html). I first attempted to simply pass this value straight to `Date.parse()` like I would with something in UTC:

```java
def dateString = "2020-10-17T02:00:21.644-04:00"
def dateObject = Date.parse("yyyy-MM-dd'T'HH:mm:ss.SSSZ", dateString)
```

Attempting this gives a really nice error message:

> Unparseable date: "2020-10-17T02:00:21.644-04:00"
> 
> Groovy telling me to get bent

After more digging than I care to admit, I finally got to the root of the problem: Groovy doesn't like the colon in the timezone offset. Rather than `04:00` it wants `0400`. Given that I work in the western hemisphere, I _could_ have taken the easy route and simply removed the last 3 characters, replacing `:00` with `00`. It's not the most elegant solution, though, and there are parts of the world with timezone offsets in fractions of an hour; assuming it's always going to be a whole hour isn't safe. My first take, written last week, was to be extremely complicated by splitting the string into a list at each colon. From there, I concatenated the list items back into a string, separating them with colons, until attaching the very last piece, which is added sans colon:

<script src="https://gist.github.com/jfabry-noc/ce2fca9ce8edf8b59dda07b0d635dda1.js"></script>

It technically works but doesn't look so good; no one would ever call the above solution "elegant" by any stretch. That was done late on a Friday, though, and over the weekend I had a better thought pop into my head. What if instead of spitting up the entire string I simply:

1. Verified first that the end of the string matched what was expected.
2. Took a substring that was everything up until the problematic part of the offset.
3. Took a substring for the numbers after the last colon.
4. Put the parts from 2 and 3 together.

After shaking off a bit of regular expression rust, this is what I ended up using:

<script src="https://gist.github.com/jfabry-noc/c35f8103841c01e8f5d20b203d505a6c.js"></script>

The partial regex match of `:\d{2}$` validates that the string ends with a colon followed by any 2 digits. If so, I first create a substring from the first index (0) up to the index at the length of the string minus 3, meaning I omit the colon and last 2 numbers. Then I append to that a substring from the overall string's length minus **2** through the end of the string. Using the overall string's length minus 2 rather than 3 means I just leave out the colon.

Is this the best solution? Probably not, but it's getting the job done in my situation. If I ran into issues, my next attempt would be to get the index of every colon in the entire string and then replace the very last one with nothing.
