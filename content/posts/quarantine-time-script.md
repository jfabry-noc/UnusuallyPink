+++
title = "Quarantine Time Script"
date = "2020-07-27"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["bash", "quarantine", "shell"]
tags = ["bash", "quarantine", "shell"]
+++

Are you tired of sitting at home wondering how many days you’ve been choosing to quarantine like a responsible adult? Me too! The number of times I’ve been in conversations or working on posts for blogs or social media and thought, “Wait, how long have I been at home now?” followed by wasting time doing rough calendar math in my head was enough that I finally burned some time this weekend putting together a script for it.

In the interest of full disclosure, every time this has come up before I’ve done some very simple PowerShell to actually calculate this, at least once I passed the point where I couldn’t just think of it off the top of my head:

```
$now = Get-Date
$then = Get-Date -Date "2020/03/11"
($now - $then).Days
```

Clearly this is extremely simple! I’ve found myself needing a few shell scripts, though, so I figured it would be a good opportunity to write this in Bash instead for a little exposure. The biggest key was to just figure out how the heck to:

1. Create a date at a specific time.
2. Subtract the dates.

## Date at a specific time

This was pretty easy after a quick [DuckDuckGo](https://duckduckgo.com) search. The `date` utility includes a `-d` parameter that allows me to give it a string that it’ll use as the date, just like `-Date` does in PowerShell.

## Subtract the dates

The second piece also ended up being _much_ more straightforward than I expected. The `date` utility similarly includes a few codes I can use to specify how I’d like the date to be formatted, including `%s` which will give the date in seconds relative to the [Unix epoch time](https://en.wikipedia.org/wiki/Unix_time). I could get both dates in seconds, subtract the current date from when I started quarantine, and then convert the seconds to days. For those keeping score at home, there are 86,400 seconds in a day.

As an added bonus, `date` returns the time in seconds just like everything else in the universe that isn’t Java-based. I’m looking at you, [Groovy](https://failti.me/posts/groovy_prog/).

## Getting a starting date

The easy method would’ve been to hard-code the date when I started quarantine and leave it at that. To make it a little more extensible, though, I instead opted to pass the date as a parameter. Given that people can pass anything as a parameter, though, I put together a regex to enforce the YYYY/MM/DD format on whatever is typed. That being said, I still included an additional check after parsing the starting date regardless since it would still be possible to specify a date that matches the regex but that isn’t real (e.g. 2020/02/31.)

## Code

Here's the code in all of its janky glory.

<script src="https://gist.github.com/JFFail/093100bdeead1850b5c9895962a5c882.js"></script>

It’s extremely simple, but it was a fun little learning experience to kill some time on a weekend when I was sitting at home… continuing to quarantine…
