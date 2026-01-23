---
title: "Web Hosting Distinctions"
date: "2021-07-08T14:15:00-04:00"
author: "Ryan Robinson"
---

There’s a confusing thing that has come up with several clients who don’t know all the intricacies of website work (completely understandably; that’s why they pay me). When we talk about hosting services around website work, there are a few different things we could be referring to: the domain, the DNS, the website content, and email. To make it more confusing, sometimes all of them are with the same provider and other times they’ve divided up in different combinations.

## Domain

The first step in the chain is registering the domain. Once you’ve got the domain registered, you own it and can do whatever you want with it. For example, I own the domain alliterationapplications.com that I bought through a particular domain registrar that I like.

The important things to know about the domain registrar:

- Keep it renewed. You’ll have to pay a recurring fee to keep it or you risk everything associated with it going offline and then potentially somebody else swooping in and stealing it. Depending on the registrar, you can often buy multiple years at a time. Usually there is an option to auto-renew when it is ready to expire, but even that does require some attention that your payment details stay valid (i.e. credit cards expire every few years).
- WHOIS privacy is a great idea, especially if you’re not a big company. This hides your contact details from the public.
- Set the nameservers to point to the DNS provider. This may be in the same place as the domain registrar or may be elsewhere, like where the website content is hosted.

## DNS

The Domain Name Service specifies details about what services are associated with the domain. The big two of these are the next items for clarification: email and website content. But there could be other things as well, like the Microsoft Endpoint Manager service for device management.

DNS related to email should have at least two components:

- MX records: these specify where email sent to the domain should be directed. This could be Microsoft, Google, a web server, or something else.
- TXT record: a particular type of TXT record called an SPF record can be used to help differentiate legitimate email sent from the domain vs illegitimate. When somebody receives an email that says it from your domain, their email provider looks up the SPF on the domain. It compares what the SPF record says to where the email actually came from to determine if the sending server is allowed to send on behalf of that domain, and is likely to mark it as spam if there isn’t a match. It isn’t the only way to help make sure your emails don’t go into spam, but it’s the simplest action for the biggest impact.

DNS related to website content is typically a simple A record pointing to the IP address of the web server. Some DNS providers also have a FWD record type which will forward all browser traffic to a different URL, e.g. my ryanrobinson.ca domain currently forwards to my LinkedIn (although I think I will merge it together with this site at some point).

You rarely need to change your DNS – only when you change those other services – and there typically is no recurring fee, with your DNS hosting most often combined with either your domain hosting or website content hosting.

## Website Content

The content of your website, usually consisting of several files and at least one database, reside on a web server. I won’t dive into all the considerations choosing a web content host, but if you’re talking about a simple site like this one, you don’t need anything special.

There will always be some recurring fee for website hosting, although it may get combined into a package with domain or DNS. If the server is well-managed by a good support team, then you won’t need to worry about much other than keeping the account active. If it is a web server where you have to manage it yourself, you will need to pay attention to details like security updates and PHP versions.

## Email

Finally, you may or may not have email associated with your domain. This email might be hosted on a web server (the same as the website content) or could be somewhere else like Google or Microsoft.

If it’s on a web server with the website, it’s probably part of the package or a small fee extra. It is very low-quality email compared to Microsoft or Google: low storage limits, no sync of contacts/calendar, bad spam filters, etc.

If the email is hosted elsewhere like Google or Microsoft, you do have another recurring fee (unless you’re a non-profit getting it for free), but you get much higher quality email as well as a variety of other services like cloud storage which can provide huge productivity advantages.
