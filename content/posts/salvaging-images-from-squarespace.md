+++
title = "Salvaging Images From Squarespace"
date = "2020-09-13"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["cdn", "powershell", "squarespace", "wordpress"]
tags = ["cdn", "powershell", "squarespace", "wordpress"]
+++

I wrote previously about [moving this blog from Squarespace to WordPress](https://unusually.pink/unusually-pink-migration/). One of my cited concerns with Squarespace was being locked into that particular platform without a lot of options for moving somewhere else. So how did I move my content to WordPress? I was able to export the _written_ content for the posts themselves from within Squarespace, fortunately. Inside of Settings > Advanced is an **Import / Export** option. The _only_ export offering is WordPress, so I guess it was lucky that's where I was moving. This gives an XML file with the written content and metadata for each post. Unfortunately, there is no option to export the _images_ that I've uploaded over the past year of creating content over at Squarespace; within the XML file the images show up as `<div>` tags with a link to the Squarespace CDN for the actual image. For example, this is what I see where the image is for the last post I authored over on Squarespace:

```
<div style="padding-bottom:45.903255462646484%;" class=" image-block-wrapper has-aspect-ratio " data-animation-role="image" > <noscript><img src="https://images.squarespace-cdn.com/content/v1/5cabd40b755be258403ccb99/1595366630913-VD12AA9IHURXLT6CWQ66/ke17ZwdGBToddI8pDm48kKEtlFlv-8yggmb8KJA0a9wUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYxCRW4BPu10St3TBAUQYVKcpEap199WJ5tA07nqy9HB7RsfdGE2RUqSBzw535kCng92V_tkyiZ3FgjXcK6wugnz/html.png" alt="html.png" /></noscript><img class="thumb-image" data-src="https://images.squarespace-cdn.com/content/v1/5cabd40b755be258403ccb99/1595366630913-VD12AA9IHURXLT6CWQ66/ke17ZwdGBToddI8pDm48kKEtlFlv-8yggmb8KJA0a9wUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYxCRW4BPu10St3TBAUQYVKcpEap199WJ5tA07nqy9HB7RsfdGE2RUqSBzw535kCng92V_tkyiZ3FgjXcK6wugnz/html.png" data-image="https://images.squarespace-cdn.com/content/v1/5cabd40b755be258403ccb99/1595366630913-VD12AA9IHURXLT6CWQ66/ke17ZwdGBToddI8pDm48kKEtlFlv-8yggmb8KJA0a9wUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N_N4V1vUb5AoIIIbLZhVYxCRW4BPu10St3TBAUQYVKcpEap199WJ5tA07nqy9HB7RsfdGE2RUqSBzw535kCng92V_tkyiZ3FgjXcK6wugnz/html.png" data-image-dimensions="1013x465" data-image-focal-point="0.5,0.5" alt="html.png" data-load="false" data-image-id="5f175ce6cb20a366ea6f4d62" data-type="image" /> </div>
```

If you think that looks disgusting, that's because it is. When I imported the XML file into WordPress, I saw an option to download any attachments on each post. I checked that box, but since the images are linked to the Squarespace CDN they're considered to be HTML content rather than attachments. As a result, WordPress simply embeds the `<div>` in each post as a custom HTML block that doesn't actually render the image.

![](/images/cash_money.png)

Set on **not** going through 50 posts to manually save the images out of them, I started looking at the XML to see if I could do anything useful with the image URLs. One thing that immediately concerned me was that, when I wasn't sure what I was going to do with the `unusually.pink` domain but knew that I didn't want to keep it at Squarespace, I marked the Squarespace site as Private, meaning the only way to view the content was to log in. I _assumed_ this meant the image content on the Squarespace CDN would be inaccessible until I made the site public again. After copying an image URL from my XML file, though, I saw that it was still publicly available. Flagging a Squarespace site as private means you can't load the site directly, but content on Squarespace's CDN is still accessible. That in itself seems like a problem to me and a very good reason to leave the platform, but in this one case it was working to my benefit. I realized that I could parse all of the images files out of the XML file with a script and download them programmatically.

As you can see from the XML snippet above, images on the Squarespace CDN have URLs like this:

```
https://images.squarespace-cdn.com/content/v1/5cabd40b755be258403ccb99/1595366630913-VD12AA9IHURXLT6CWQ66/ke17ZwdGBToddI8pDm48kKEtlFlv-8yggmb8KJA0a9wUqsxRUqqbr1mOJYKfIPR7LoDQ9mXPOjoJoqy81S2I8N\_N4V1vUb5AoIIIbLZhVYxCRW4BPu10St3TBAUQYVKcpEap199WJ5tA07nqy9HB7RsfdGE2RUqSBzw535kCng92V\_tkyiZ3FgjXcK6wugnz/html.png
```

There's a whole lot of CDN nonsense, followed by a forward slash and the original file name at the very end. While this would be handy for getting the original file name, I didn't want to end up with dozens of images in a folder where I had no idea what post they belonged to, and I definitely didn't want to manually correlate the file name with the CDN link in each of the HTML blocks in WordPress.

The XML, though, also includes the title of each post, and I realized that if I was scanning each line of the XML for image tags, I could also check for the title tag and keep a variable constantly updated with that. With this idea, I would just _start_ each file name with the post title so that they would be grouped together. Once the dots were connected, it was simple to come up with the following short PowerShell script:

<script src="https://gist.github.com/jfabry-noc/10fd04fd8aa85e710e3acab5b0e55889.js"></script>

It downloads all of the images referenced in the XML file in the format of:

```
postTitle_fileName.extension
```

I added some extra checks to remove unsavory characters from the file names; while it's a valid character in most modern filesystems, for example, have you ever tried to actually programmatically do things from a Bash shell with a file that has a `[` in the name? It's not pretty.

While this saved me from having to manually download each image from Squarespace, I still had to manually go through each post in WordPress, remove the custom HTML block where each image should have been, and then upload the appropriate image. With the way I downloaded the images, though, I just started at the top of the directory and worked my way through the images alphabetically since each post was grouped together. It sucked, but it could have been a _lot_ worse. If nothing else it made me glad that I moved forward with migrating the site now rather than waiting a few more months for the Squarespace subscription to lapse; I didn't want to deal with this for any more posts than was strictly necessary.
