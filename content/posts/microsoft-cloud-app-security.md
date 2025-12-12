---
title: "Microsoft Cloud App Security"
date: "2021-06-10T07:05:00-04:00"
author: "Ryan Robinson"
series: MS-101
tags:
  - "Microsoft 365"
  - Security
---

If you’re an IT admin, do you know what apps users are putting on devices alongside company data? Do you know all the apps that they are directly putting company data into, thinking it helps solve a problem for them? This is the problem of “Shadow IT.” If it’s a personal device, it’s even worse, as they might be installing all kinds of insecure apps without IT approval and it wouldn’t take much to make a mistake like copying and pasting company data or uploading a file into the wrong app.

The first lesson of this problem is that it is very important that IT provides the apps that every user needs. Users shouldn’t have to decide for themselves what to use and whether it is secure enough. If they’re forced to make those decisions instead of IT, there’s a good chance they won’t consistently make good ones.

The Microsoft Cloud App Security dashboard helps with this problem. It empowers admins to see the apps being installed on corporate devices. It also allows you to set policies around those apps to help contain data.

## Discover

Step one is discovering what apps are on the devices. The Discover -&gt; Create snapshot report will allow you to create a report based on the logs from services like corporate firewalls.

The Cloud app catalog will show all of the apps in Microsoft’s directory. There are thousands. They all come with detailed risk scores providing information on the app, including its security, and legal. For example, SugarCRM scores an 8 overall, with a 7 in general, 10 in security, 4 in compliance, and 10 in legal. That’s pretty good, but depending on the needs of your organization (like HIPAA compliance), there might be some issues to look into there. This is helpful information to decide whether to approve or deny an app for your users.

## Investigate

The Investigate section allows for checking in on reports for users, files, and activity across connected apps. You can set up connected apps in this section as well, so that apps like Office 365 report back to CAS.# PPoliciesThe policies section comes with several great policies by default. Familiarize yourself with these defaults before starting to create a new one, as there might already be something close to what you want that just needs a bit of tweaking. There’s a lot here, so I’ll run down the policy types quickly:

- *Access:*  these require first having [conditional access policies](/posts/2021/microsoft-conditional-access-policies/) configured. Then you can enforce access requirements logging in to other apps.
- *Activity:*  these monitor user activity within their apps, like logging in from a risky IP address or mass downloads (which could suggest something like a disgruntled employee grabbing as much as they can on the way out).
- *App discovery:*  these policies help you respond when a new app is discovered to be in use in the organization.
- *Cloud discovery anomaly detection:*  these help detect unusual behaviour like suddenly uploading a lot more data than in a normal workday.
- *File:*  these help with detecting potential issues at a file level, such as using [a DLP engine](/posts/2021/data-loss-prevention-policies/)  to notice when sensitive data is shared to a different app, or files shared to other domains.
- *Session:*  these provide real-time monitoring and control over what users are doing in cloud apps, while they’re signed in to that session. They also require conditional access policies.

With each policy, you can set the severity level and whether you want to be alerted by email, text message, or start off a Flow in Power Automate to respond in other ways.
