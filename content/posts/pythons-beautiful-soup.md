+++
title = "Python's Beautiful Soup"
date = "2020-08-18"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["python", "scripting", "twitter"]
tags = ["python", "scripting", "twitter"]
+++

In my [last post](https://unusually.pink/twitter-development-impressions/), I more or less just complained about what a dumpster fire developing anything for Twitter is. Originally, though, the post was intended to be about _what_ I was developing for Twitter. It’s nothing amazing or even complicated, but it was a fun learning experience… Twitter itself not withstanding. I made a Twitter bot tweeting different shades of pink each day. Since that’ll most likely seem nonsensical to most people, let me explain.

## Background

A little over a year ago, a friend and I started a podcast. I won’t go into the backstory of why we named it what we did, but the name of the podcast revolved around the color pink due to an inside joke between my friend and I. We ended up publishing 21 episodes in the span of a year before we decided to stop it. I had moved about an hour away from where I previously lived for a new job, so recording in-person involved a decent bit of travel for one of us. Then the coronavirus pandemic really started to take off in my country, and given what a dumpster fire trying to record a podcast remotely is, my friend and I jointly decided to shutter the podcast. It was a fun experience, but nothing either of us were really wanting to keep putting time and money into. As is typically the case, we reached this conclusion just a month after the hosting for both the podcast and our website renewed. Go figure.

That being said, we had set up social media for the podcast, and that social media was now doing exactly nothing. While I didn’t want to do anything with the Facebook or Instagram accounts that my co-host ran (you couldn’t pay me to touch a Facebook property), I thought about what I could do with the lingering Twitter account. I eventually decided to make a simple bot that would tweet a different shade of the color pink each day.

## Python and Beautiful Soup

As I [mentioned previously](https://unusually.pink/twitter-development-impressions/), the actual code to post to Twitter ended up being extremely simple. I just used the [Twython](https://github.com/ryanmcgrath/twython) library to do the heavy lifting. What ended up being more interesting was how to create the database of colors I would use. After all, I don’t personally know that many different shades fo pink, and I wanted to include the RGB and hex color codes for each shade in the daily post. I basically needed a repository of shades of pink. After some DuckDuckGo-fu, I eventually found [a page that included not just the RGB and hex color codes, but also a _name_](https://html-color.codes/pink#:~:text=Pink%20Color%20Codes%3A%20colors%20shown%20are%20similar%20to,palevioletred%3A%20%23db7093%20%2F%20rgb%28219%2C112%2C147%29%20deeppink%3A%20%23ff1493%20%2F%20rgb%28255%2C20%2C147%29). It was exactly what I needed.

The only problem was how to get the information from that page into something I could use in my script for the bot. My immediate thought was to copy and paste all of the information, but along with being error-prone over hundreds of shades, that’s also insanely tedious. In a shell script, something like `xmllint` would fit the bill. Since I was already working in Python, though, I decided to use [Beautiful Soup](https://pypi.org/project/beautifulsoup4/). I had actually used Beautiful Soup one time before on a project years ago where I admittedly didn’t really know Python and most definitely didn’t understand what I was doing with Beautiful Soup; I just ended up copying and pasting a bunch of code from the Internet until things worked the way I wanted.

This time, I took just a little time to read the documentation for Beautiful Soup and understand what I was actually doing. The crux of my script comes down to:

```python
divisions = soup.find_all("div", {"class": "color-inner"})
```

This gets me each of the `div` groupings for a color. With each of those groupings defined, it was then simple to get the name, hex, and RGB information I needed:

```python
for division in divisions:
    color_name = division.find("span", {"class": "color-sub"}).get_text()
    color_hex = division.find("span", {"class": "color-id"}).get_text()
    color_rgb = division.find("span", {"class": "color-rgb"}).get_text()
```

Instead of trying to copy everything by hand, I had a working script to get all of the colors without needing to worry about human error. Plus, if the source website adds any new colors it’s trivial to re-run the script and get an updated list. I ended up making a map for each color and adding all of the maps to a list.

```python
rows.append({"name": color_name, "hex": color_hex, "rgb": color_rgb})
```

Then I wrapped it all up at the end by exporting the list of maps to a JSON file.

```python
with open('pinks.json', 'w') as outfile:
   json.dump(all_colors, outfile)
```

My other script which actually pushes the post to Twitter ingests this JSON file and then selects a random shade from it.

## Twitter Still Sucks

As if further proof was needed that Twitter is garbage, though, I found myself simultaneously amused and irritated just a few days ago when I saw that a daily post had not been completed. When logging into the account for the bot, I received a notification that the account had been flagged for “suspicious activity”, and I had to walk through a verification process before the account could post again. It’s amazing to me that a platform which tolerates the most hateful and dangerous rhetoric chooses to flag a clear bot that makes a single post each day with details on a different shade of the color pink as “suspicious.” It’s just further proof that Twitter really isn’t worth anyone’s time at this point.

My latest project, though, involves pushing data to Mastodon instead of Twitter. This post will serve as the first test of it, so assuming everything works look for a post on that in the near future.
