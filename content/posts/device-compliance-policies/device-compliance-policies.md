---
title: "Microsoft Endpoint Manager: Device Compliance Policies"
date: "2021-05-31T11:16:00-04:00"
author: "Ryan Robinson"
description: "An introduction to device compliance policies."
series: MS-101
tags:
    - "Microsoft 365"
---

[Once you’ve got devices enrolled in Microsoft Endpoint Manager](/posts/2021/enrolling-devices-endpoint-manager/), one of the very useful things you can apply are compliance policies. These provide you a way to monitor and enforce restrictions on devices which are not following the proper practices that you want in your organization.These compliance policies can be set up for devices of multiple operating systems:

- Android
- iOS
- macOS
- Windows 10 and later
- Windows 8.1 and later

As is the case elsewhere in Endpoint Manager, Chromebook is the noticeable omission.

The exact options from there will vary depending on the platform, but they break down into three decisions: how do you define non-compliance (compliance settings), what happens if it’s not compliant (actions for noncompliance), and who is bound by this compliance policy (assignments).

## Compliance Settings

This tab defines what it takes to be considered compliant vs non-compliant. Windows 10 has the most options and I won’t list them all, but some that stand out include:

- **Require Bitlocker**: encrypted hard drive
- **Minimum and maximum OS version**: this is a great way to make sure that updates are happening, e.g. you may want to make sure nobody is on a version older than a year
- **Firewall**
- **Antivirus**
- **Antispyware**
- **Microsoft Defender Antimalware**, including **Real-time protection**
- **Microsoft Defender for Endpoint**: require that the device is under a certain score if it is enrolled in Defender for Endpoint

In general, I suggest that you at least require Bitlocker, firewall, antivirus, antispyware, and Defender antimalware with real-time protection. There are a lot of variables, e.g. if you are using a different anti-virus solution instead, but I think that’s a good baseline.

!["Some of the device compliance settings, including requiring Bitlocker"](./device-compliance-settings.png)

## Actions for Noncompliance

The next question is what you want to happen if the device violates the compliance rules. There are a few options:

- **Mark as noncompliant**: you’ll always want this, but you get an option of how long before that happens. That decision will depend largely on other settings. For example, if this is the only consequence, it can be a good way to monitor violations to mark noncompliant immediately. But if you’re pairing it with something like conditional access, you don’t want to end up immediately locking out a user from company resources because they were a little slow updating Windows. In that case, you’re likely better off sending an email warning right away and not marking noncompliant for a couple of weeks.
- **Send email to end user:** this is a helpful tool if the factor not in compliance is something that the end user can easily fix themselves. You can even define a message template to provide instructions on how to fix it.
- **Retire the noncompliant device:** this will remove company information from the device. This should be the last resort only if the user did not get the device back into compliance quickly enough.

!["Possible actions when device is not compliant"](./device-compliance-actions.png)

## Assignments

Finally, there are options for who this compliance policy applies to: all users or specific groups. You may want to apply a more lenient policy to everybody, then have more strict policies for users like senior management who have access to the most sensitive information.

!["Options for who this policy is applied to"](./device-compliance-assignments.png)
