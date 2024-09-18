---
title: "Visual Studio Code: Remote SSH Development"
description: "An overview of connecting to SSH servers using VS Code."
date: "2021-04-12T10:27:00-04:00"
author: "Ryan Robinson"
tags:
  - "Visual Studio Code"
---

One of the greatest improvements to my website development workflow came the day I discovered I could directly code on a web server in Visual Studio Code. Before this realization I was opening files with FileZilla, which worked but took a few clicks for each change:

1. Browse to file in FileZilla
2. Double-click to prompt opening in Code
3. Switch over to Code, make changes, and save the file
4. Switch back to FileZilla
5. Say yes on the prompt to upload the change
6. Test in browser to see if it did what I wanted it to do
7. Repeat 3-6 if necessary for next change

By comparison, when you access the file directly in Code, that process gets cut down to:

1. Open file in Code, make changes, and save the file
2. Test in browser to see if it did what I wanted it to do
3. Repeat 1-2 if necessary for next change

If you’re making a lot of changes like testing out new CSS designs which may take several iterations to get just right, all those extra clicks add up to a lot of time. It’s also easier to remember; something that happened for me regularly is I saved the file in Code but forgot to go back to FileZilla to upload the file, then was briefly confused why my changes weren’t showing up yet.

## Setup

I won’t go step-by-step in getting this setup. There are [lots of great blogs](https://towardsdatascience.com/5-steps-setup-vs-code-for-remote-development-via-ssh-from-windows-to-linux-b9bae9e8f904) out there to walk you through it.

## Peacock extension

[This extension](https://marketplace.visualstudio.com/items?itemName=johnpapa.vscode-peacock) applies a different colour theme to my workspace for each website I was connected to. This is really valuable especially if you find yourself working on multiple websites on the same day as the visual cue helps avoid accidentally updating the wrong site.

You can manually set these for each site, which could be used for scenarios like setting the code window to match the branding of that site for an even stronger visual cue, or you can check the box to surprise me with a random colour whenever one isn’t already set.

## Git integration

[I wrote another recent post that breaks down how Git integration in Code in general](/posts/2021/github-visual-studio-code/). The main difference here is how it ties into the next point: if you can’t use the integrated shell because of the security on the system, you also won’t be able to push in the version control interface.

![Source control window showing the files staged and not staged yet, with interface options for everything you need.](../github-visual-studio-code/source-control.png)
_Demo of the source control window_

## Shell integration

This is not something I can take advantage of due to security settings of the web server I mostly work on, but it is a great idea if your server allows it. With this feature, you can directly use your shell commands within Code instead of needing a separate window in something like PuTTY or the Windows Subsystem for Linux (WSL).
