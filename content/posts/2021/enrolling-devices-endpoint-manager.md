---
title: "Microsoft Endpoint Manager: Enrolling Devices"
date: "2021-05-12T04:11:00-04:00"
author: "Ryan Robinson"
description: "How to enroll devices in Endpoint Manager."
series: MS-101
tags:
  - "Microsoft 365"
---

Suppose you’ve started to move toward managing your devices in Microsoft Endpoint Manager (Intune). [There are a lot of methods available to do that](https://docs.microsoft.com/en-us/mem/intune/enrollment/device-enrollment). I’ll highlight just a few of the most interesting:

## Windows Autopilot

If the device was set up with Windows Autopilot, enrolling to Endpoint Manager is one of the options to happen immediately as part of the setup. No further actions are necessary.

## Auto-enroll

Auto-enrollment makes the process very simple. If you’ve set up automatic Mobile Development Management (MDM) and/or Mobile App Management (MAM) for all users or for a group that includes that user, then as soon as the user signs in on the machine with their business account, the device is added to Endpoint Manager.

There are some other settings around this including whether they have to agree to a terms of use when they first sign in.

## Device Enrollment Manager

If you prefer a more manual approach, you can designate specific staff (e.g. your IT team) as Device Enrollment Managers. Users with this permission can enroll up to 1000 devices each.

## Co-management

If you have an on-premise network with Configuration Manager, you can use co-management to split MDM duties between the two systems. You can even pick which workloads you want to be handled by Configuration Manager and which you want to be handled by Endpoint Manager.

This can be a good strategy if you already have Configuration Manager and want to transition to Endpoint Manager one workload at a time, or could be a more permanent solution if you want to keep certain workloads on Configuration Manager.
