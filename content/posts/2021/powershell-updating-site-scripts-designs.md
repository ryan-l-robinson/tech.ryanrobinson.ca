---
title: "PowerShell: Updating Site Scripts and Designs"
description: "Part of the SharePoint Site Provisioning project, here are some scripts for updating site scripts and designs from PowerShell."
date: "2021-04-05T11:20:00-04:00"
author: "Ryan Robinson"
series: "SharePoint Site Provisioning"
tags:
  - Microsoft 365
---

This post begins a series on [SharePoint site provisioning](/tags/sharepoint-site-provisioning/), unpacking some of the problems I’ve faced and overcome in building SharePoint site provisioning solutions.

[Site scripts and site designs](https://docs.microsoft.com/en-us/sharepoint/dev/declarative-customization/site-design-overview) are a great feature of SharePoint. They allow for developing and using templates on SharePoint sites that can do many useful things like:

- Create a list or library
- Apply column or view formatting on a list or library
- Apply a site logo image

There is one annoying limitation, though. These scripts and designed are entirely created through PowerShell. That can make it a pain when you’re testing out a new script and need to update it frequently with each change, especially if you’re handling multiple scripts at once.

So I wrote some PowerShell that handled some basic logic for me:

- [Connect to the Microsoft 365 account](https://docs.microsoft.com/en-us/powershell/module/sharepoint-online/connect-sposervice?view=sharepoint-ps)
- [Check if the script already exists](https://docs.microsoft.com/en-us/powershell/module/sharepoint-online/get-spositescript?view=sharepoint-ps)
- If yes, [update that script](https://docs.microsoft.com/en-us/powershell/module/sharepoint-online/set-spositescript?view=sharepoint-ps)reading from the latest file with an incremented version number
- If no, [create a new site script](https://docs.microsoft.com/en-us/powershell/module/sharepoint-online/add-spositescript?view=sharepoint-ps)
- [Check if the site design already exists](https://docs.microsoft.com/en-us/powershell/module/sharepoint-online/get-spositedesign?view=sharepoint-ps)
- If yes, [update that design to include the relevant scripts](https://docs.microsoft.com/en-us/powershell/module/sharepoint-online/set-spositedesign?view=sharepoint-ps), with an incremented version number
- If no, [create a new design to include the relevant scripts](https://docs.microsoft.com/en-us/powershell/module/sharepoint-online/add-spositedesign?view=sharepoint-ps)

The code is available on [my GitHub account](https://github.com/ryan-l-robinson/SharePoint-Site-Update). The current version is minimal, but I may return someday to make some more updates to it like how to automate multiple scripts on the same design.

With this PowerShell script, you only need to tweak it to fit your scheme, and then run the PowerShell each time you need to upload your site script’s changes to your SharePoint tenant. The README file on GitHub provides more detail.
