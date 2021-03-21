+++
title = "Adding SSH To GitHub"
date = "2020-12-23"
author = "John Fabry"
authorTwitter = "UnusuallyPink"
showFullContent = false
keywords = ["git", "gitcommit", "https", "netlify", "ssh", "website"]
tags = ["git", "gitcommit", "https", "netlify", "ssh", "website"]
+++

Between the Thanksgiving holiday and some shifting priorities when I got back to work, I hadn't made any new commits to my [GitHub account](https://github.com/jfabry-noc/) for about 10 days. This meant that I didn't have any new posts to my recently-launched [GitCommit](https://gitcommit.gay) site that makes a bit of a microblog from my commit messages. To be honest, putting it together was really an excuse to have _something_ up at the domain I bought, but I really like the way it turned out. After making a fresh commit yesterday, though, I received an email from GitHub telling me:

> You recently used a password to access the repository at jfabry-noc/OxidizedBackup with git using git/2.25.1.
> 
> Basic authentication using a password to Git is deprecated and will soon no longer work.

It also included a link to their [authentication requirements documentation](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/). On the whole it made sense to me; I typically would just have `git` remember my credentials so that I never had to enter them. In the new setup, I would _have_ to enter them, though I could configure how long they would remain cached. On the surface, this didn't bother me, and I went about my day making several additional commits beyond the first. Later on, though, I realized that my new commits weren't appearing on the GitCommit site. The site is hosted by [Netlify](https://netlify.com), and it works by polling my GitHub account via a `cron` job every 4 hours (I'm hoping to eventually update it to use webhooks to have commit information pushed to it, but I haven't had an opportunity yet.) When a new commit happens, my code pulls the repository, the commit message, and the timestamp. It then updates the HTML and automatically makes an _additional_ commit on its own repository (yes, I included code to avoid an endless repost loop.) Netlify has been configured to look at only the **html** directory in the repository, dynamically updating whenever a new change is commited to that directory, much like how [GitHub Pages](https://pages.github.com/) or any other CI web hosting works.

Since my new commits weren't showing up, I first checked the [GitHub repo for GitCommit](https://github.com/jfabry-noc/GitCommit). No new HTML changes had been commited to it. Next, I went to the VPS which executes the `cron` job. For my own sanity, I long ago learned that I should implement some type of logging for any code that I plan to run automatically so that I can easily troubleshoot it during this exact situation. Checking the log file, I saw that my code _did_ successfully find the new commits and update the appropriate HTML file. It was able to make the expected commit, and running `git status` even confirmed that my local repository was ahead of the remote by 2 commits. However, running the following didn't work to push my changes up to GitHub so that Netlify could see them:

```shell
git push origin main
```

Suddenly it clicked, and I realized that the change from earlier in the day was the culprit; my code could no longer `git push` because that was now prompting for a password that would never be typed given that the code was executed by `cron`. To confirm, I manually executed the script from my SSH session and saw the password prompt appear. Ick.

Fortunately, I realized that I wouldn't need a password if I was syncing my repository via SSH rather than HTTPS. SSH would allow me to use certificates instead. First, I had to generate a new public-private key pair with:

```shell
ssh-keygen -t ed25519 -C "email@email.com"
```

Yeah, I'm not telling you my email address. I'll give you a hint; it's something@unusually.pink. Good luck. The above command asked for a couple of things such as the location where I wanted to store the keys and if I wanted to add a password for accessing them. I just took the defaults. Next up, I had to start a background SSH agent and tell it to use the private key I had just generated:

```shell
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id\_ed25519
```

With everything set on the client side, I now had to tell GitHub to use my public key for any SSH interactions. They have [very solid documentation](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/adding-a-new-ssh-key-to-y) available with the exact steps. Just be sure you actually copy the content of the **public** key into GitHub rather than the private key. GitHub won't accept the private key because the formatting is wrong, but the very fact that I pasted it there made me paranoid enough to regenerate the pair and do it again.

To test things out, I tried to clone one of my private repositories via the SSH link, which you can select from the tabs when trying to clone anything from GitHub.

![](images/github_ssh.png)

That worked successfully and verified that my keys were properly identifying me. The last step was to update the local repository for GitCommit to use a new URL for the remote repository. Regardless of whether I'm using HTTPS or SSH, I still run `git push origin main` in order to sync it to the remote; the configuration of the local repository is what specifies which URL is used. In order to flip mine from HTTPS to SSH, I simply ran the following from my local directory for GitCommit:

```shell
git remote set-url origin git@github.com:jfabry-noc/GitCommit.git
```

After this, I re-ran my script and `git push` was able to successfully push my code to the remote without any interaction. After working through many commits today, I verified that my website was updated as expected.

As a bonus note, astute readers may recall the issues I had following the published steps to sync the [Dracula theme for git](https://unusually.pink/all-dracula-everything/'). This was the exact same scenario where the instructions assumed that SSH was in use rather than HTTPS.
