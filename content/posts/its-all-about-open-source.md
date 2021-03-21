+++
title = "It's All About Open Source"
date = "2019-05-06"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["open-source", "software"]
tags = ["open-source", "software"]
+++

A friend of mine who has been doing some programming (hi, [Fy](http://fyritke.us/)!) recently asked me for some ideas on open source projects they could look at and possibly contribute to. It made me think of what open source software I tend to use on a regular basis… which is a decent amount as someone who runs a few different Linux systems. It seemed like a solid opportunity to compile the Super Official List of Open Source Stuff John Likes™. As someone who doesn’t really know much about writing code, I have **no** idea if any of these are good candidates for someone to start contributing to open source; they just happen to be things I use on the regular. Without further adieu and in no particular order:

### [Notepad++](https://notepad-plus-plus.org/)

Notepad++ has been one of my favorite Windows text editors for as long as I can remember. While it may not be as flashy or as popular as some of the other editors I’ll get to in a little bit (yeah… I’ve got a thing for text editors), it’s hard to top the speed and footprint of Notepad++. It also has at least basic syntax highlighting for almost every programming language under the sun. I also can’t overstate the importance of being able to open a plaintext file at work and having the editor launch _immediately_.

### [Visual Studio Code](https://code.visualstudio.com/)

VSCode is another [rare example of Microsoft software I enjoy](https://www.unusually.pink/blog/microsoft-edge-insider-its-actually-not-that-bad). While I dislike the large, clunky IDE that is Visual Studio, VSCode is a nice balance between a simple editor and an IDE. It has a large plugin ecosystem that can give you benefits of things like Intellisense and a debugger without being needless. It’s based on the same Electron framework as [GitHub’s Atom editor](https://atom.io/). I’ve found it to be such a nice editor across a wide array of languages that I’ve even taken to installing it on Linux systems for the times when I want a GUI editor.

If you’re curious how I configure it, here’s my **settings.json** file. Note that I mainly use it for PowerShell:

{
    "workbench.colorTheme": "Dracula",
    "editor.minimap.enabled": true,
    "editor.minimap.showSlider": "always",
    "editor.wordWrap": "on",
    "editor.scrollBeyondLastLine": false,
    "files.hotExit": "off",
    "powershell.integratedConsole.showOnStartup": false,
    "editor.fontFamily": "'Fira Code', 'Fira Mono', monospace",
    "editor.fontLigatures": true,
    "git.ignoreMissingGitWarning": true
}

### [Vim](https://www.vim.org/)

Of course, most of the time when I’m using Linux I’m **not** using a GUI. I feel at home on the command line, which is good considering I run a handful of headless servers that I only ever access via SSH (which happens to be a **great** way to mess with scripting or code from a Chromebook if it doesn’t have support for Linux apps yet.) Vim is the de facto CLI editor for me (sorry Emacs users), and I can install it on literally everything. Most Linux distros also come with either it or Vi installed by default, so it’s ubiquitous.

Vim has a reputation for being difficult to use, but I don’t really think that’s the case. It’s just that it’s very _different_ to use if you’re coming from most modern editors. Once you get accustomed to it, I’ve found it to be perhaps the most easy for editing text effectively. Plus, it has a rich plugin ecosystem for things like syntax highlighting and support. I use [Pathogen](https://github.com/tpope/vim-pathogen) as my plugin manager for Vim.

If you want to know my Vim config, this is my typical **.vimrc** file:

execute pathogen#infect()
set hlsearch ignorecase smartcase incsearch relativenumber ruler
set laststatus=2 tabstop=4 shiftwidth=4 expandtab notitle
syntax on
filetype plugin indent on

### [htop](https://hisham.hm/htop/)

htop is a _super_ handy, interactive utility for seeing what the heck is happening in a Linux system. Think of it like the terminal version of the Windows Task Manager. It gives you a nice breakdown of CPU, RAM, and swap usage along with a listing of processes and what each is doing. It also offers and easy way to adjust the nice level of particularly important or greedy processes. It’s an enhancement over the older **top** utility.

![](/images/ItsAllAboutOpenSource_Screenshotfrom2019-05-0617-32-51.png)

### [tmux](https://github.com/tmux/tmux)

tmux is terminal multiplexer, hence the name. If you have no clue what that means, it allows you to take a single terminal and divide it up into multiple virtual terminals. This lets easily have multiple terminals on the screen at the same time with different information on them without having to flip between tabs. Here’s a sample:

![](/images/ItsAllAboutOpenSource_Screenshotfrom2019-05-0618-16-41.png)

In a single terminal window I’ve got Vim open with some simple Go code on the left pane. The right side has two panes; the top pane has Cowsay running while the bottom pane I just used to install Cowsay. While not exact useful in the scenario I set up for this screenshot, it can be really handy for doing something like writing a script in one pane and having a second, smaller pane to the side or top of running it periodically without ever needing to close the file.

The other super handy part of tmux is that you can keep a persistent session going on a remote system without staying connected to it. I can SSH to a server, open tmux, connect to an IRC server, and use it for however long I need. If I want to disconnect from SSH but keep my IRC session going, I can simply detach my current SSH session from tmux but leave tmux running. Then I can close my SSH session. When I later SSH into the system again, I can reattach to the existing tmux session and pick right back up where I left off.

### [PowerShell](https://github.com/PowerShell/PowerShell)

Since I’ve mentioned code a few times in these examples after starting the post off by saying I don’t know what the hell I’m doing when it comes to writing code, PowerShell is the one exception. I basically live in a PowerShell window for work, using it for both my day-to-day management at the CLI and in scripts that I use to automate my work… because work smart, not hard, right? I feel decently proficient with PowerShell, and I’m excited that version 6 is now open source! It’s nice to be able to use the same scripting language and commands to manage Windows servers at work that I use to manage my Linux servers at home.

I haven’t posted anything new in a while (I’ve written _tons_ of stuff but just haven’t thought to post it), but you can see some of my sample PowerShell scripts over on [GitHub](https://github.com/jffail). I’ve also posted some [/r/DailyProgrammer](https://www.reddit.com/r/dailyprogrammer) challenges as [Gists](https://gist.github.com/jffail).

### [Hugo](https://gohugo.io/)

Hugo is a static site generator. The concept is that instead of needing a CMS (content management system… think something like WordPress) to manage posts, pagination, design, etc. on a website that you can instead do it all via plaintext. Hugo allows you to have HTML templates, CSS, and then posts that are authored as Markdown. When you make a new post or change the site in any way, you can recompile your site which is then output as simple HTML and CSS that you can throw onto a web server. New post? Recompile the site and just move the files. Hugo worries about things like how many posts there should be on a page and will adjust it all for you. Need to change information in your header? Just change it one time in your template file and then recompile; there’s no need to use **sed** through every page and change each of them.

There are plenty of other static site generators out there ([Jekyll](https://jekyllrb.com/) is a popular one), but I’ve found that Hugo is by far the fastest. When your site starts to get large with a lot of pages to parse and generate, generators like Jekyll — which is written in Ruby, an interpreted language — can start to bog down. Hugo is written in Go; it’s literally one binary, and that allows it to be super speedy even when your site is large.

### [NetHack](https://github.com/NetHack/NetHack)

To end the post on a fun note, NetHack is an incredible video game. It’s easy to look at it and assume that it’s a simplistic, basic game. It runs in a terminal (though variants with tiles and graphics do exist), and everything in the game is represented as an ASCII symbol. Your character? The @ symbol. A kobold? The letter k. The game is also crazy old… it was released in 1987. Here’s what it looks like:

![](/images/ItsAllAboutOpenSource_Screenshotfrom2019-05-0618-38-44.png)

It’s a fantasy game so the whole point is to hack-and-slash your way through a procedurally generated dungeon, meaning no two games are the same since each level is random; on top of this is the fact that there are more classes, races, and mechanics than most modern games have.

Even better is that the game is _still being updated_. The latest commit on their GitHub repo was yesterday. Pretty nuts. The game is also a marvel of what’s possible in the C programming language. Imagine making something like this without even being able to use objects.

NetHack exists for every operating system on the planet, but if you don’t want to bother with installing it you don’t have to. You can instead just hop on the [alt.org NetHack server](https://alt.org/nethack/).

There are, of course, tons of other open source applications I use on the regular — I didn’t even bother getting into operating systems — but these are the ones I use the most frequently and enjoy the most. The most important thing, though… is to **stay pink!**
