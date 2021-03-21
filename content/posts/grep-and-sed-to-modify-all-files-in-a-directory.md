+++
title = "Grep and Sed to Modify All Files in a Directory"
date = "2020-08-02"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["bash", "grep", "sed", "shell"]
tags = ["bash", "grep", "sed", "shell"]
+++

I recently decided to rebrand one of my websites, complete with a different domain, title, and author (that’s me!) This is part of the beauty of using a static site generator like [Hugo](https://gohugo.io/); I updated the domain in my configuration file, and everything else just magically changed when the site was recompiled. The caveat is that I wanted to change the `author` attribute in each post to a different name to match my [new Mastodon profile](https://fosstodon.org/@consoleaccess). In this particular [Hugo theme](https://themes.gohugo.io/hugo-theme-terminal/) the author is specified in each post rather than in the central `config.toml` file so that I can have different authors in a single site. This meant I needed to mass modify all of my posts to change the author (at least for the ones that have it since I previously used a theme where the author wasn’t specified.) I knew that this should be possible with `sed` but couldn’t remember the exact syntax. Since Hugo stores all of the Markdown files for my posts in a single directory, though, I knew it shouldn’t be too complicated.

The syntax `sed` uses to do find-and-replace is _very_ familiar to me since I use the same syntax in `vim` all the time. It’s great for those moments when I realize I’ve given a particular variable or function an extremely poor name and need to change every instance in a particular file. So what I really needed in this case was to recall how to use the CLI to change the value in multiple files; specifically I needed to change every file in a particular directory. It didn’t take many searches for me to confirm that this would require another utility to discover each file so that `sed` could then update them. Ultimately I got up and running with the following commands thanks to [what I found on this site](https://isaacsukin.com/news/2013/06/command-line-tip-replace-word-all-files-directory):

```shell
grep -lr -e "author = \"oldName\"" . | xargs sed -i '' -e "s/author = \"oldName\"/author = \"consoleaccess\"/g"
```

In this case, `grep` is discovering the files and then I’m using `xargs` to redirect the output from `grep` to the arguments for `sed`. I won’t rehash the specific parameters of each command, as the original site I linked to above does a terrific job of that. However, it’s worth mentioning that I was able to swap out _just_ the instances of my old account name for the Markdown file’s **author** property by specifying the full line instead of just the account name; all I had to do was use the `\` to escape things like the double-quotes that I needed to include. This way everything runs as efficiently as possible since `grep` is only returning the files that contain the old name in the author property, and then `sed` is only changing that single line of each file passed to it.
