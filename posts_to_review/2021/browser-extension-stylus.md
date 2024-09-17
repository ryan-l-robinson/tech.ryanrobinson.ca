---
title: 'Browser Extension: Stylus'
date: '2021-09-21T11:33:00-04:00'
author: 'Ryan Robinson'
tags:
  - Accessibility
---

The [stylus extension](https://chrome.google.com/webstore/detail/stylus/clngdbkpkpeebahjckkjfobafhncgmne) is a new addition to my browser ([Edge](/posts/2021/web-browser-edge-chrome/), available for all Chromium browsers). The core idea: it allows for changing stylesheets on websites while you browse without actually changing the server.

This might be because it’s a website you don’t own. Get really annoyed at the new Twitter font (I actually really like it)? Write your own stylesheet so that whenever you’re browsing you see something else.

It also can be a good technique for trying out a change over a period of time without actually needing to change the stylesheet on the server. This can simplify potential conflicts like others in your team editing at the same time or being confused by your in-progress changes.

## How it works

You can write your stylesheet directly in the Stylus extension, including specifying which domains the change should apply to for you.

![](/assets/img/2021/09/Stylus-Editor.png)
_Stylus editor with a single change to font-size_

Turn on the stylesheet in the extension and begin your regular testing. If you’ve worked with CSS on complex sites before (e.g. writing your own stylesheet on top of a WordPress or Drupal theme), you have probably encountered a scenario where the change you made applied it either too narrowly (missing a spot) or too broadly (changed something you didn’t mean to). Where Stylus can really be an advantage is that you can continue to browse the site as normal while working on other things. That gives you more opportunity to identify the problem before you actually save the change to the server. If there’s a problem, simply edit the CSS again and go back to browsing.

Once satisfied, you can add the CSS to the site’s server for real.

## Do I need it?

Whether you need this extension or not depends on the nature of your workflow and your team. If you are the only person making changes on a development server, you probably may as well edit directly on the server right away instead of using Stylus for an extra test tool. But if others might also need to edit on the server, or if you want an extended period of time where you can test it without others seeing a change, then Stylus is a nice tool for the toolbox.