---
title: "Considerations Migrating IMAP to M365"
date: "2021-06-16T09:46:00-04:00"
author: "Ryan Robinson"
tags:
  - "Microsoft 365"
---

Email migrations are one of those tasks you won’t have to do often, but you’ll really want to make sure you’re doing it correctly when it does need to happen. As much as collaboration tools like Microsoft Teams and Slack have taken over a lot of communication, email is still a central component of many organizations.

There are lots of permutations of migrating email from one service to another. In this post I’ll focus on one I’ve done a few times and is more likely with smaller organizations: from a web server to Microsoft 365. These smaller organizations often set up their email on the same server as their website many years ago. Those email services are usually quite bad – slow, low storage limits, bad spam filtering, emails might sync across devices but contacts and calendar don’t, etc. – but maybe they were good enough for a long time and it’s a pain to switch. The need for a migration might also come up when the organization wants to move their website hosting and suddenly realizes the flaw in having their email hosting connected to the same account.

I’m not going to break down the detailed steps of a migration. These are well documented by Microsoft. Instead, I’m going to mention a few potential complications to be aware of.

## Plan Your Time

Some of the steps take time. You won’t do this all in one day. But if you plan it properly, it can be a seamless transition for the users. So plan out what your steps will look like, which might be something like this:

Day one: create the new accounts in Microsoft 365. Take a look at the migration process to make sure you’ve got a good understanding. Especially if you haven’t done it before, do a trial run with just one of the email accounts. Put out the request for all the credentials you need, including the IMAP server details and the passwords of current users. Make sure to collect these securely, using tools like [QuickForget](https://quickforget.com). Also make sure you have any alternate emails for users in case you need to contact them while they don’t have their work email.

Wait a couple of days to get all the passwords.

Day three: prepare the spreadsheet with all the user names and passwords. Start the migration process.

Day four: check in to see how the migration went. Ask a few users to confirm that all their content copied correctly. Notify everybody of the time that their email will change and what they need to do to access it, with some detailed instructions of how to log in to their new accounts and add them to Outlook. They can try to access their new accounts already – they exist, just might not have all the content yet.

Day five: once you’ve confirmed everything is copied over well, change the DNS records to point all new email to Microsoft 365 instead of the IMAP server. This could take from minutes to a day depending on the DNS server. Do this outside of business hours if possible, so the users never notice the in-between period where some emails go to the old mailbox and some to the new. Let the users know that they should start accessing from Microsoft 365 any time now, reminding them of the instructions.

Day six: confirm everybody is successfully accessing the new mailbox, providing added training or support as necessary.

You don’t have to follow this exact timeline. It’s just a demonstration to point out there will be gap times where you have to wait between steps, so plan accordingly.

## POP and IMAP

Make sure people aren’t accessing via POP. If a user is checking their email only using Outlook on a single computer with the POP protocol instead of IMAP, changes they make on their computer (like folder structures) do not sync back to the server. The migration process is server-to-server, so this means that you end up migrating the unsorted mailbox on the server instead of the nicely organized structure of the computer. That’s not good.

In this case, the best solution may be to use the PST export and import in desktop Outlook to migrate this mailbox.

- Walk the user through exporting their mailbox as a PST.
- Either walk them through adding their new account in Outlook, or have them send you the file and you can do it for them.
- Import the PST of the old mailbox into the new mailbox in Outlook.
- Wait for everything to sync back to the Microsoft server, possibly hours depending on the mailbox size.

It’s a slow but straightforward process, not too onerous as long as you don’t have to do it for a lot of users.

## Contacts and Calendar

IMAP migrations handle email, including folder structures. They don’t include contacts and calendar. Users have to do that manually. If the users want to migrate those things (they may not, depending how they use them), make sure they have instructions for how to export their address book and calendar and then import to the new Microsoft mailbox.

## Training

I’ve already mentioned this, but don’t forget training. If you spend a lot of time in Microsoft 365 like I do, it might be easy to overlook that you’ve just set up accounts for people who may have never used Microsoft services before. You’ll need to make sure they know everything they need to know. That includes things like:

- How to check their new email: on the web, desktop Outlook, mobile Outlook or other app
- Installing the desktop Office apps if they’re starting those subscriptions at the same time, which is likely
- Some core Microsoft 365 admin functions like adding new users, groups, and resetting passwords for a couple of their trusted admin users
