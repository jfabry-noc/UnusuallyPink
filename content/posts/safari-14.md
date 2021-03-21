+++
title = "Safari 14"
date = "2020-10-04"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["apple", "browser", "privacy", "safari"]
tags = ["apple", "browser", "privacy", "safari"]
+++

Last week the 10.15.7 update to macOS Catalina came with a nice surprise: Safari 14. I was caught off guard by this since I had assumed we wouldn't see Safari 14 until [Big Sur released later this year](https://unusually.pink/thoughts-on-apples-wwdc-2020/). It was also a nice surprise for me since Safari has become my browser of choice, not just on my iPhone and iPad, but also on my MacBook Pro. The big reason for this is that I do my best to avoid any [Chromium](https://en.wikipedia.org/wiki/Chromium_(web_browser))\-based browser. Over the last few years we've seen diversity in browsers erode more and more as new browsers are built based on Chromium (e.g. Brave) while others abandon their own engines in favor of using Chromium (e.g. Opera and Edge.) I personally see this homogeneous browsing platform as being pretty bad for the Internet as a whole, as it opens up the possibility for web developers to focus all of their development on Chrome and ignore everything else. This leads to sites that _only_ work on Chrome and that ignore web standards, just like we saw back in the day when much of the web was developed with only Internet Explorer 6 in mind. The difference now is the way the web has evolved into an entire platform. In 2004 the main issue was that sites developed just for IE 6 wouldn't quite render properly on other browsers. In 2020, there are entire web apps that straight up won't work on non-Chromium browsers. That's something I can't support.

The two _major_ browsers moving forward with different engines are Firefox (with Gecko) and Safari (with WebKit.) I was previously using Firefox on my laptops, but I became extremely concerned recently when [Mozilla had massive layoffs and switched their mission to focus on revenue](https://blog.mozilla.org/blog/2020/08/11/changing-world-changing-mozilla/). I certainly understand that Mozilla needs to make money in order to continue making Firefox, but when a group lays off their entire incident response team, I don't exactly feel warm and fuzzy inside about using the product. I still use it on my Linux installations, but on macOS I switched to Safari.

The pleasant part about switching to Safari is that, for the most part, it's been a very slick browser that I've enjoyed. While Safari 14 doesn't do anything too Earth-shattering or even different from any other browsers, it does bring Apple's offering up to parity with some of the major players. For example, Safari will now _finally_ display favicons for websites on tabs. How they've made it this far without supporting them I'll never understand, but it immediately makes a huge difference in quickly finding the tab I want... and I say this as a person who typically doesn't have more than 10 tabs open at any given time. Tab addicts (you know who you are) will especially appreciate this when Safari starts stacking tabs on top of one another. As another update to tabs, Safari can now preview the content of a page when the mouse is hovered over a tab. This can also be useful for quickly finding the appropriate tab without actually having to switch to anything.

The big change, though, is how Safari communicates with the user about how it has helped protect against invasive tracking. This feature is _extremely_ similar to the Protections Dashboard in Firefox. There's an icon to the left of the address bar that can be clicked at any given time to see a breakdown of trackers on the current page. Clicking will also allow me to see the specifics of what trackers are being blocked:

![](images/safari_icon.png)

For a bigger picture, I can also get an overall view of what's been blocked in the past 30 days. I can see which sites were attempting to be the most invasive, and similar to the per-site rendering, each can be expanded to show which trackers they had embedded:

![](images/safari_websites.png)

Similarly, I can click on the **Trackers** heading in order to see a list of which trackers appear the most frequently across the sites I'm visiting. I can expand those listings to see which specific sites are hosting that tracker:

![](images/safari_trackers.png)

I don't think it should come as a surprise to anyone that Google, Bing, and Facebook appear the most frequently after just a short period of testing. It's also interesting to see trackers from both Facebook and Snapchat when I don't use either of those "services". It really shows you how pervasive they are across the Internet.

While I can already hear the Apple-haters I know railing on the fact that Firefox already has this feature, in my opinion it's nice to see Apple bringing their browser up to feature parity and offering a more transparent and secure browsing experience to people in a package that _also_ does not leverage Chromium but which _does_ have a support team behind it that's more than a skeleton crew. Similarly, you _still_ don't see anything like this today in Chrome or Edge, likely because the companies behind them both appear relatively high up in the tracker list.
