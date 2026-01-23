---
title: "Microsoft Teams: Guest Notifications"
date: 2022-03-03T16:44:00.000Z
author: Ryan Robinson
description: "Understanding guest notifications in Teams."
tags:
  - Microsoft 365
---

Microsoft Teams has the great ability to invite users who are in other organizations to join your Teams in order to collaborate, assuming your IT admin has not turned that feature off. Guests in the Team can’t do everything that an internal member can do, but it’s pretty close, so it’s a strong environment for connecting with partners.

But there is one pretty significant catch to be aware of: notifications.

Here’s a way to think about it that I find helpful: it comes down to who owns the data. When somebody messages in a Team channel thread, that data is owned by that Team.

If you message in a Team, notifications will pop immediately for anybody currently in that tenant, whether internal or guest.

A message will not pop for users who have guest access to that team but are not currently operating in that tenant (probably because they’re in their own tenant). Notifications do not cross tenants. What they do get is the same as if they missed a notification in their own tenant: an email summarizing that a notification is waiting for them about an hour later, and a notification waiting for them in their Activity tab the next time they sign in to the relevant tenant.

So here’s the real question: suppose I am in my tenant, which does include a guest user, and I want to get that guest user’s attention immediately. I don’t want to wait for the email notification to go out, or for them to think to check in to my tenant’s notifications. How do I do that?

The good news is that the presence indicator is tenant-specific. Anywhere you see their profile, like when you tag them within a channel thread or you try to start a chat with them, you can see if they are online in your tenant.

If they are, it’s easy to proceed with messaging within your tenant to them as a guest.

If they are not in your tenant, the next option is an external message instead. External and guest aren’t the same thing. External sends a chat message (not a Team channel thread) from your tenant to theirs. Guest messages them within your tenant and it can also use the full Team channel collaboration tools. When you message somebody in Chat, you can see whether they are Guest or External, and you can have the same contact as both Guest and External at the same time, which can be confusing to remember which to message when.

So, if they aren’t online in your tenant and you have to reach them in a hurry, the best thing you can do is send a message to the external user asking them to switch over to your tenant. Then you can continue the conversation in the Team channel.

It can be unintuitive at first, but once you’ve got the basic idea of guest vs external, it does make sense.
