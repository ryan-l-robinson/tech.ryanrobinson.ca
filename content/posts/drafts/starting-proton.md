---
title: "Decisions on Software Categories"
date: 2026-02-24T13:18:09.000Z
author: Ryan Robinson
description: "My experiences with Proton email so far, including some feature losses."
series: "Big Tech Alternatives"
tags:
  - "Tech and Society"
---
I've begun more deeply exploring moving my primary email from Outlook to Proton, in my attempt to rely less on US Big Tech that can use the money they make from my data to do things like bribe authoritarian governments while also fully subjecting themselves to their every whim.
## The Losses

Proton doesn't include task management at all. There are lots of good task management apps, but it does mean another app with another account, not at all integrated into my calendar. That's not ideal.

On desktop, I can use the Proton app. It also means that I can't manage my separate Outlook account in the same app.

I could instead use the Proton email in Outlook using an IMAP connection. That mostly works well enough for syncing the email component. It doesn't sync the calendar or contacts at all. There would be a significant cost in functionality if I want that convenience of having both in the same app.

On mobile, it doesn't connect to device contacts at all. That's a potential big loss. It is noted in a documentation page that they plan to do that.

It also doesn't connect to the device calendar at all. So you can't even have one calendar from Proton and one from Outlook and then look at them from the same app, not without publishing one of them to share through the cloud to the other.

### TBD

How's the spam filtering? Considering Proton doesn't have any access to the contents of the email, just the metadata including the subject, is that noticeably worse?

### Positives

The obvious big positive is the privacy. If you're sending to another Proton user, it's fully end-to-end encrypted. That probably isn't too many people you know, but they realize and they still have some other mechanisms to make sure the message is encrypted entering their services and they cannot access it at all.

There's also an option if you really need to email somebody something really private, which you normally shouldn't, that you could put a password on that message so it will stay encrypted without that password. I'm not sure I'd use that. I would probably be more likely to try to get the secure information communicated in other ways, but maybe it would come up as a good option after all.

Aside from privacy, I will say first and foremost that I love how simple and clean the interface is. It's just so friendly to use. Maybe this is because it doesn't have as many of the deep features that most of us have forgotten are in Outlook anyway, but sometimes less is more.

The extra "hide-my-email" addresses are a wonderful idea as well. You can give out these fake emails to big company lists that you may not really want to have your real email. Messages from them will still come to you, but now if they get hacked and that email gets out, it isn't a big loss. You could even delete it and start up a new one, effectively unsubscribing from every service that you gave that address to.

The custom domain for the email I've already touched on a little in the previous post in this series. It's great to control your email address. If I decide I don't want to be on Proton anymore, I could go back to Outlook - I didn't delete the account, just stopped the email's DNS from sending to it - or I could go to some cool new Canadian provider that doesn't exist yet. I have that freedom because I own my domain and have tied my email to it. When your email is @gmail or @outlook, it's a lot harder to move without needing to tell everyone who may ever email you again, a task daunting enough that many will never bother.
