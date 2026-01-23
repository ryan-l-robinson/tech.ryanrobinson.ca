---
title: "Microsoft Endpoint Manager: Device Configuration Policies"
date: "2021-05-26T03:38:00-04:00"
author: "Ryan Robinson"
description: "An introduction to device configuration policies."
series: MS-101
tags:
  - Microsoft 365
  - Security
---

[You’ve got your devices enrolled in Endpoint Manager](/posts/2021/enrolling-devices-endpoint-manager/). Now what? This opens up lots of tools including configuration policies.

Configuration policies allow for quickly rolling out the desired configuration to the device, without the user having to manually set it up. This can include a lot of different settings and vary by the operating system of the device. Some of the more interesting tools for Windows 10 includes:

- Email, contacts, calendar, and tasks (add the account corresponding to the user logged in)
- Restrict apps to only those from the Store
- Block access to system settings
- Password requirements
- Start menu
- Battery saver settings
- Edition upgrade (from Pro to Enterprise)
- Domain join
- VPN
- Wi-Fi
- Windows Hello for Business

!["Options for configuring a VPN on Windows 10"](./vpn-device-config.png)

VPN particularly stands out to me in the age of COVID-19 and lots of people working from home. Setting up a VPN is not something that is intuitive to most users. With a device configuration profile, they don’t have to figure that out themselves or lose time letting an IT staff member remote access to set it up.

That’s not a comprehensive list. Ultimately, you can do a lot which helps save the user time configuring themselves and/or helps enforce some security blocking them from features they don’t need.
