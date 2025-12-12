---
title: Mail Tester
date: 2021-12-04T01:35:34.000Z
author: Ryan Robinson
description: "If you test any system sending mail, mail-tester is an essential tool."
tags:
  - CRM
---

A common scenario with many web services like a CRM is sending mail and realizing that it is often going to the junk of recipients.

There are several reasons why a message may go to spam. Some of those reasons come from the content of the message. Some are because of filters set up by the recipient or their email provider. And some factors come down to technical configuration on the part of the sender.

It’s this last group which can be aided by a tool like [mail-tester.com](https://www.mail-tester.com/). It is simple to use:

1. Visit the site and copy the unique email it gives you.
2. Send a message to that address. You want this test to be as close to your real scenario as possible. If you’re trying to test from a website, send from the website. If you’re trying to test from Outlook, send from Outlook. And so on. Similarly, make the content a copy of a real message.
3. Return to the site and hit the “Then check your score” button.

It will provide you with a report including a score. If you’re hitting at least an 8 or 9 out of 10, you’ll be fine sending to most email services. It is true that some services weigh factors differently, though. Most of the recommendations it will give to improve your score are not that hard to implement for an IT person, so aim for everything you can.

It is also true that sometimes email providers have their own block list which you could have ended up on through no fault of your own, e.g. maybe your website has a similar IP to a spam offender and the provider blocked a bunch of addresses including yours along with the real offender. A good sign that this is happening is if you notice patterns, like if Gmail never receives your messages but Outlook does, or vice versa. If the latter happens with Gmail or Outlook and you’ve done all the other standard things that mail-tester can help you identify, you can file a request with them to get off the list.
