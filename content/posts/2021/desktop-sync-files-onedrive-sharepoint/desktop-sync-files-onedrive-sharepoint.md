---
title: "SharePoint: Desktop Sync Files"
description: "The differences between SharePoint and OneDrive, and which approach to use for desktop sync, can be confusing. Here's some help on the advantages of each."
date: "2021-04-02T07:18:00-04:00"
author: "Ryan Robinson"
tags:
  - Microsoft 365
---

## What Goes Where?

The first question to consider when planning a file structure in Microsoft 365 is what files go where. That’s more complicated in an online cloud collaboration system than it is for one person on the computer. It needs to make sense for everybody and it needs to be able to easily maintain proper permissions.

My typical guidance is this: it comes down to who owns the file. If the file is just for you, go ahead and put it in your OneDrive with whatever folder structure you want. But if the file is for others to also access, it should be in SharePoint with a group owning it.

That can make it a bit less intuitive for individual users to still have easy access to everything they need, which might now be scattered across their OneDrive plus a handful of SharePoint libraries.

The files can be accessed in multiple ways, like in OneDrive in the browser, in SharePoint in the browser, or in Teams. But this post will compare two different strategies for accessing files on your computer, for those who are most comfortable having content synced to their machine and browsing in Windows Explorer.

## Sync SharePoint Libraries

The older strategy is to sync each SharePoint library separately to the computer. To do this, navigate to each library in your browser, hit the Sync button in the toolbar, and follow the prompts to sync on your computer using the OneDrive client.

The end result looks like this:

!["Communication site – Documents library synced separately"](./sharepoint-library-synced-separately.png)

It’s a completely different folder on your computer from your OneDrive.

The biggest advantage of this over the next option is that there is a clear visual difference. It’s easy to remember which files are just yours in OneDrive and which are shared in SharePoint. That helps avoid scenarios like deleting a folder without realizing that deletes it for everybody, not just you.

## Add To OneDrive

A newer feature is the “add to OneDrive” option which appears in the toolbar when browsing a SharePoint location. Instead of syncing it separately, it will treat the shared file as if it was part of your OneDrive, while still maintaining all of the group permissions.

The end result looks like this:

!["Communication site’s documents are now in the business OneDrive alongside individual documents"](./sharepoint-library-added-to-onedrive.png)

Everything is in one place, which many people like better than needing to remember which files are OneDrive and which are SharePoint. There is a little link icon in the status bar, indicating this is a shared library and not owned by that OneDrive, but that might be obvious enough to everybody.

The other big advantage is that you won’t have to do this again on every device. The link is kept in your OneDrive, so even as you move between machines, you’ll only need to sync your OneDrive. If you’re on one machine all the time, that might not matter much, but even if you’re regularly switching from mobile to PC, it’s nice to know everything is always where you need it in OneDrive.
