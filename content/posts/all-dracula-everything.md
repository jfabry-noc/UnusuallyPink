+++
title = "All Dracula (Theme) Everything"
dat = "2020-05-31"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["atom", "dracula", "dracula-theme", "firefox", "vim", "visual-studio-code"]
tags = ["atom", "dracula", "dracula-theme", "firefox", "vim", "visual-studio-code"]
+++

When you spend a significant portion of each and every day staring at a computer screen, it's important for things to look nice. Aside from just aesthetics, it can help reduce eye strain and improve productivity. With that in mind, we wanted to help spread the word about one of the best themes ever created: [Dracula](https://draculatheme.com/). I _believe_ I first learned about the Dracula theme from someone I follow on Twitter, though enough time is passed that I'm not 100% positive on who or if that's even where I really found it.

Dracula is a beautiful dark theme with a focus on pink, green, red, and white to give it a striking visual style that's also easy on the eyes; it's nice when working with code to see things stand out so clearly. One of the strengths of Dracula is also that it feels like it exists for _everything_.

## [Terminal.app](https://draculatheme.com/terminal)

![](images/AllDraculaThemeEverything_terminal.png)

I spend a **large** portion of my day working out of Terminal.app on my MacBook Pro, and the Dracula theme fits with it perfectly. While you end up with a lot more white text here than in most text editors, the smooth color scheme still makes for something a bit more pleasing to look at than the stock green text on a blackbackground that I've used in my terminal emulators for decades.

#### Bonus

If you aren't using [zsh](https://www.zsh.org/) as your shell already, you should definitely consider switching to it!

## [Vim](https://draculatheme.com/vim)

![](images/AllDraculaThemeEverything_vim.png)

Vim has been my text editor of choice for years now. While the learning curve is steep compared to a lot of other text editors, it's honestly not too difficult to get started with the basics. Once you've got those covered, it's easy to simply look up what you need when the need arises to expand your skills with it. After a little time, you can quickly become much more productive; I'm now significantly _slower_ when navigting plaintext with a combination of arrow keys with Ctrl, Home, and End.

### Caveat

The directions on the Dracula site for installing the theme with my favorite Vim plugin manager, [Pathogen](https://github.com/tpope/vim-pathogen) aren't quite correct and assume you've done a few steps that many people will not have done. The directions instruct you to do this:

git submodule add git@github.com:dracula/vim.git bundle/dracula

This may give you the following error:

fatal: not a git repository (or any of the parent directories): .git

The error is _very_ generic, so searching the Interwebs might not give you relevant results. The tl;dr is that you can't clone a submodule via git into a directory that isn't already a git repository. So if you're in the `~/.vim` directory you can remedy that by running:

git init .

I _still_ got an error after running the original command, though:

git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

I didn't have git set up in a way to allow me to clone with that format, or maybe you're intended to change the "git@github.com" to your own GitHub user information? I'm honestly not sure, but rather than screwing with it I just changed the URL to the https URI for the repository:

git submodule add https://github.com/dracula/vim.git bundle/dracula

This cloned without any issues.

### Bonus

If you like my font in the screenshots above, I recenetly switched to using [Cascadia Code](https://github.com/microsoft/cascadia-code) from Microsoft. It looks terrific, is extremely readable, and works well in both text editors and the terminal. While I've stopped using it in favor of Cascadia Code, [Fira Mona](https://mozilla.github.io/Fira/) from Mozilla is also an extremely strong font if Cascadia isn't your thing.

## [Atom](https://draculatheme.com/atom)

While I do most of my work in Vim, occasionally I'll find myself wanting/needing a GUI-based editor. In those cases, my editor of choice is Atom from GitHub. It has a solid built-in package manager with a wide array of official and community-created plugins for language support.

## [Visual Studio Code](https://draculatheme.com/visual-studio-code)

This one is more Brandi's thing than mine, but Visual Studio Code is another terrific GUI text editor. It also has the best [PowerShell plugin](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell) of any text editor, making it the de facto PowerShell editor if that's what you work in (even though I still use Vim with [vim-ps1](https://github.com/PProvost/vim-ps1).) Both Visual Studio Code and Atom are [Electron](https://www.electronjs.org/) applications, so you get a similar footprint and responsiveness with either of them. I say that as a warning since Electron apps can be a little on the slow side when launching them. More on that later.

## [Firefox](https://draculatheme.com/firefox)

![](images/AllDraculaThemeEverything_image-asset.png)

Hugo kudos to Brandi for discovering this one, but there's even a Dracula theme for Firefox! As a privacy-focused individual, Firefox is my browser of choice on all of my laptops. I've spent years using and being happy with the default dark theme in Firefox, but Dracula just takes it to the next level. I really appreciate how subtle the theme is with just hints of pink you see around active menus or the currently active tab.

## Honorable Mentions

#### [Notepad++](https://draculatheme.com/notepad-plus-plus)

Back when I was using a Windows laptop for my job, I'd do a lot of my code editing in Atom and/or Visual Studio Code. They're great editors, but Electron apps can be a bit on the slow side given that you're loading the Chromium runtime engine for it. Notepad++ was what I used as a backup when I just needed to quickly open a plaintext file for quick viewing and/or edits rather than a prolonged coding session.

#### [Windows Terminal](https://draculatheme.com/windows-terminal)

Another holdover from when I used Windows for work, you may recall the [podcast episode where we raved about the new Windows Terminal](https://www.unusually.pink/podcast/episode-6-huzzah-hobbies). While I no longer have a Windows 10 machine, if I did you can bet that I'd be using the Dracula theme with it.

Much like the elements in the Dracula theme... stay pink!
