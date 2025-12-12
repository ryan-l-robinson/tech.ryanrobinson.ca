---
title: "Microsoft 365: Email Suspected of Spam"
date: 2022-05-13T01:35:47.000Z
author: Ryan Robinson
description: "How to respond to Microsoft 365 emails being blocked as suspected spam."
tags:
  - Security
  - Microsoft 365
---

## What’s the Error Message

A scenario I encountered recently was a user getting this error message:

> Your message couldn't be delivered because you weren't recognized as a valid sender. The most common reason for this is that your email address is suspected of sending spam and it's no longer allowed to send messages outside of your organization. Contact your email admin for assistance.

This happened with every email they tried to send.

## Why it Happened

They were not actually sending spam in the truest sense of that word. But they were sending bulk emails out of Outlook like any other email, and possibly without some of the requirements we have in Canada for anti-spam legislation (e.g. there has to be a link to unsubcribe from any bulk mailing list). So, even if it wasn't truly spam, it had some of the characteristics of spam: sending a lot at once, with the same generic message, without the ability to unsubscribe. Microsoft's email service concluded that this was spam, probably because you got hacked, and thus shut down the account until an administrator could investigate.

The valuable thing to remember here is that Microsoft 365 email mailboxes are not intended for bulk mail distribution. There are specific services like MailChimp that are designed to handle all those extra factors in a way that a standard email doesn't. They will send out in staggered time, with multiple servers. They'll have templates that ensure you aren't missing any legal requirements. So, don't use a standard email - Microsoft or otherwise - to send bulk mail. It will catch up to you eventually.

## How to Fix It

After this error occurs, an administrator with the necessary permissions will have to unblock the user's account. Don't do this until you're confident that you've dealt with the root issue, i.e. that the sender is not sending bulk email anymore.

Once you're ready:

1. Go to [the restrictured users section in the security centre](https://security.microsoft.com/restrictedusers).
2. Follow instructions available in [official Microsoft documentation](https://docs.microsoft.com/en-us/microsoft-365/security/office-365-security/removing-user-from-restricted-users-portal-after-spam?view=o365-worldwide).
