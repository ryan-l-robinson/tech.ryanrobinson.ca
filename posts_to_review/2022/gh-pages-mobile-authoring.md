---
title: "GitHub Pages: Mobile Authoring"
date: 2022-05-16T02:47:58.000Z
author: Ryan Robinson
category: Websites
tags:
    - Static Sites
    - Git, GitLab, and GitHub
---

[I’ve recently switched from hosting these tech posts on a WordPress site running on a shared host I pay a small fee for, over to GitHub Pages for free](/posts/2022/building-jekyll/). It’s a great system and might be all you need if you’re a developer wanting to write and publish some content quickly.

One thing I was worried about, though, is writing from my phone. My habit has often been that I write rough notes on my phone, then finalize it on a computer and hit publish. I didn’t want to lose that. So here’s what I did.

## Mobile App 

First I needed an app on my Android phone to edit the Markdown files. I tried a few and settled on [Markor](https://play.google.com/store/apps/details?id=net.gsantner.markor&gl=US). It can stay opened to an entire folder, can create files, edit files, delete files. You can write in markdown and preview what it looks like (without anything like CSS styling from the site, of course). It does have some modes that are unnecessary to me - to do and quick notes - and no way to turn them off in the interface.

The runner up was [WriterPlus](https://play.google.com/store/apps/details?id=co.easy4u.writer&gl=US). It was a cleaner interface. It's more of a pure distraction-free writing app. But it did not have syntax highlighting, WYSIWYG buttons, or a preview mode. Those aren't big losses for my purposes mostly of writing notes on the phone to be finalized on a computer, but I decided Markor was worth the trade.

## Sync

The next problem is getting those markdown files to the GitHub repositories. One app, [GitJournal](https://play.google.com/store/apps/details?id=io.gitjournal.gitjournal&gl=US), did promise to do this directly from the phone, although I did not go all the way through tests for that. But I wasn't looking for that. As I said in the intro, I usually like to write from my phone but then finalize it from a computer. So I was happy with a middle step of getting my notes to a computer and then using the computer to get it to GitHub.

That brought me to [the OneSync app](https://play.google.com/store/apps/details?id=com.ttxapps.onesyncv2&gl=US). This lets me sync the folder of the whole project to my OneDrive. I can write on my phone and then pick up later on a computer or vice versa.

## Commit to GitHub

After that is straightforward. The files are on my computer in a project folder via OneDrive. Now I can open that folder in [Visual Studio Code](/tags/visual-studio-code/), finalize the changes, and commit to GitHub which will rebuild the site based on the latest files. Publishing only one article at a time is simple enough if you’re familiar with git: you can stage and commit that file while leaving others uncommitted as “drafts.” or I can keep drafts in a different folder, in which case they can be committed without publishing. 

I've gone as far as having separate folders depending on the stage of what I'm writing: there are ideas for new drafts, there are drafts where I have started but not finished the necessary demos and tests, there are drafts where everything is ready but not finished writing, and there are posts that can be published. All but posts go in the gitignore so they stay synced in my OneDrive but not in my GitHub.
