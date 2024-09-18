---
title: "Microsoft Endpoint Manager: Windows Autopilot"
date: "2021-05-19T16:08:00-04:00"
author: "Ryan Robinson"
tags:
  - Microsoft 365
  - MS-101
---

Windows Autopilot is a great system for deploying new Windows 10 devices, especially in the age of COVID-19 and so many working from home. [Here’s the official documentation breaking down the details](https://docs.microsoft.com/en-us/mem/autopilot/windows-autopilot).

The high level overview is that the user of the machine receives it, perhaps at home or perhaps in an office. They turn it on. Depending on the configuration options the admin has set up, they may have as few as two things they need to do to get the device ready for use:

1. Connect to the Internet.
2. Login with their business Microsoft 365 email.

This is much more straightforward than a typical Windows 10 setup, helping get around all those extra questions that usually come up. It also takes care of adding some key points of configuration, including connecting the device to Azure AD, enrolling in Microsoft Endpoint Manager, and restricting the creation of admin accounts (if you’re effectively being a remote admin).
