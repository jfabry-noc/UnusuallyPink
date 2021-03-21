+++
title = "Groovy Programming: Creating an ISO Date at a Specific Date and Time in UTC"
date = "2020-06-08"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["date", "datetime", "groovy", "iso", "programming", "utc"]
tags = ["date", "datetime", "groovy", "iso", "programming", "utc"]
+++

I'll be the first to admit that the title of this post is a **bit** too verbose, but if someone else had a post online with a title like this it could've potentially saved me a lot of time. Here's to hoping it helps someone else in their search.

As is mentioned on my [personal website](https://jfabhd.com/), I do a lot of PowerShell and Python scripting for work. Recently, though, I started working in Groovy due to the fact that it's treated as a first-class language in a system I've been integrating SaaS applications into. Without going into a big tl;dr, working in Groovy affords me a lot of convenience, security, and speed benefits. I just had to, you know, learn Groovy. Since there was a sale at Udemy at the time, I ended up snagging [The Complete Apache Groovy Developer Course](https://www.udemy.com/course/apache-groovy/). It wasn't a bad course in getting me up to speed; I went from never using Groovy (though I did use Java, upon which Groovy is based, _many_ years ago in college) to being able to get things done for work with it in about 4 nights of binge-watching the course. Be warned, though, that some of the material is a little dated. For example, the way to build JSON objects covered in the course seems to have been superseded by a far easier method, and when covering REST API requests the instructor opted to use some janky 3rd party library that at the time of this writing hadn't been updated in 6 years or something crazy. I was able to do some searches online and leverage Java tutorials to just figure out how to natively handle REST API calls; your mileage my vary with that depending on your comfort level with REST APIs.

One thing not covered in the course that I needed, though, was how to create a date object in a specific format (ISO) at a specific time (the first day of the current month) in a specific timezone (UTC.) This is a common need for me when filtering time-based events if true time UUIDs aren't available. The key in my experience is to figure out how to get the timezone correct at the creation of the object when you need a specific hour; adjusting the timezone after the fact will result in a new offset that'll make your hour value incorrect.

Being super out of the loop with Java and very new to Groovy, this was a little tricky for me, and it seemed like no one else I was able to find online had my specific need. So after a couple of hours of throwing spaghetti at a virtual wall (i.e. copying and pasting lots of stuff from [Stack Overflow](https://stackoverflow.com/)) I eventually got to this; it's probably [easier to read at the Gist link](https://gist.github.com/JFFail/dabd6ab1334fb00bfe5e089d8072ac3c):

```java
TimeZone.setDefault(TimeZone.getTimeZone('UTC'))
Integer year = Calendar.getInstance().get(Calendar.YEAR)
Integer month = Calendar.getInstance().get(Calendar.MONTH)
def then = new GregorianCalendar(year, month, 1, 0, 0, 0).time
// Desired format: 2020-06-01T00:00:00.000000+00:00
thenISO = then.format("yyyy-MM-dd'T'HH:mm:ss.SSSZ", TimeZone.getTimeZone("UTC")) 
thenISOFixed = thenISO[0..thenISO.length() - 3] + ":00"
println thenISOFixed
```

I essentially needed to take the current month and year in order to create a date object at midnight (00:00:00) on the first of the month. I need that to be in UTC, and the resulting string representation needs to be in ISO format. All of this gets me what I need so that I can plug it into an API URL for time-based filtering.

Everything works pretty much like you'd expect, though the format specified on line 6 gives me the offset for UTC as `0000`. The endpoint I was passing the data to wanted it to be `00:00`. Since I knew I'd always have no offsett, I just took a range of the string that cut off the last two zeroes so that I could replace them with ":00." I probably could've done this slightly fancier with some type of "00$" regex replacement, but this works fine.

I'm far from the Groovy master, so as much as I hope this is helpful to someone who finds themselves in the same situation I was in, I'd equally welcome someone showing me the better way to do it if one exists. Regardless, hopefully someone else out there will bang their head against the keyboard for a shorter amount of time thanks to this. And if you happen to know a better way, feel free to give me a shout!

Keep coding, and stay `#FF66CC`!
