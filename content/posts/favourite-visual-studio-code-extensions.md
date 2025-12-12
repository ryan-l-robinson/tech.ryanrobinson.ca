---
title: 'Visual Studio Code: Favourite Extensions'
date: '2021-09-06T21:13:00-04:00'
author: 'Ryan Robinson'
description: "These are some of my favourite VS Code extensions."
tags:
    - WordPress
    - Drupal
    - Git, GitLab, and GitHub
    - 'Visual Studio Code'
---

I’ve written before about [Visual Studio Code](/tags/visual-studio-code/), my preferred text editor. One of the great things about VS Code is the large number of extensions that are available, while continuing to be a lightweight and efficient program even with lots of extensions installed.

Here are some of my favourite VS Code extensions in my workflow:

## General

- **GitLens**: If, like most, you are using Git for version control, GitLens is a nice addition on top of [the default Visual Studio Code capabilities for Git](/posts/2021/github-visual-studio-code/). It adds features like seeing who committed what line of code on what date, within the editor as you’re going, which can inform your decisions especially when examining legacy code.
- **GitLab Workflow**: If GitLab is the home for your code repositories and you make use of some of the GitLab project management features such as issues and merge requests, GitLab Workflow can be a good addition to your VS Code installation. I track much of my task list using GitLab Issues and it is very handy to be able to see them directly alongside my code, even if with only a subset of the features you would get in the web browser (e.g. you can view all the conversation but not change details like due date).
- **TODO Tree**: Many developers leave notes to themselves and their team in the comments in the form of writing TODO or FIXME. This extensions adds a sidebar pane showing you where all of those notes are across all the files in the workspace, making it easier to return to them.
- **vscode-icons:** If you’re browsing a large file system, it can get a bit visually overwhelming to see long lists of directories. This extension helps by providing icons to differentiate more easily at a glance.
- **Prettier**: Code style is important, especially in a team. Prettier helps maintain consistent style.

## Web Development

- **Remote – SSH**: [I’ve written about this in more detail](/posts/2021/vs-code-remote-ssh-development/), but if you’re working remotely on some other server, this is essential. It’s a lot easier to work directly on one of those servers than to write on your computer and upload with every change. It even offers an integrated terminal so you don’t need a separate terminal app.
- **Remote – SSH: Editing Configuration Files**: In conjunction with the Remote SSH extension, this extension helps with syntax highlighting when you need to edit your SSH config files, which you will need to do setting up each site.
- **Apache Conf**: If you ever need to edit Apache configuration files, then Apache Conf will help by giving you syntax highlighting.
- **Peacock**: Peacock lets you change the colour palette of your workspace window. This isn’t just for attractive style. If you’re regularly jumping around between lots of servers – even dev vs staging vs production of the same site – then it can be really easy to accidentally change code or execute a terminal command in the wrong site. Having different colours for each one is a nice shortcut to cue your brain which one is which.
- **WordPress Hooks Intellisense**: Developing on WordPress? Install this extension to help with prompts and syntax highlighting for WordPress functions.
- **Drupal Syntax Highlighting**: Similarly, if you’re developing on Drupal, this extension will help with Drupal-specific syntax and functions.

For any of these, you simply need to look up the extension name in the extension directory, available within your VS Code window. In some cases you’ll also be prompted with a recommendation based on editing a file of a certain type.
