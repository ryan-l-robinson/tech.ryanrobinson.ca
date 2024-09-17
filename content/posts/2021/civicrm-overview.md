---
title: "CiviCRM: Overview"
description: "CiviCRM is a good tool for a non-profit CRM. Here's my summary of the good and the bad."
date: "2021-03-31T11:37:00-04:00"
author: "Ryan Robinson"
tags:
  - CRM
  - Security
---

The platform I worked with more than any other in my previous job was [CiviCRM](https://civicrm.org/). CiviCRM is an open-source CRM system aimed primarily at non-profits that builds on top of an existing WordPress, Joomla, or Drupal website. Drupal is the most powerful because Drupal has great permissions control already and CiviCRM can tie in to those, but the others are fine, too.

After a few years, I have a pretty good sense of the strengths and weaknesses that CiviCRM offers and will do a quick breakdown here.

## The Good

CiviCRM does two non-profit functions really well:

### Contributions

CiviCRM is great with contribution pages. If you want to add a donation page to your website, it will do that and it works well. It ties in with third-party payment processors, the most reliable and cheapest being iATS and Stripe, so they securely handle transferring the money to your account while CiviCRM tracks data about the donor and the donation. It’s smooth enough and works well out of the box, with a good set of configuration options.

### Events

Similarly, CiviCRM has some good functionality around public event signup forms. People can sign up online, including payment through a payment processor, and all the data about the event and the participants will be saved in CiviCRM. It even includes functions like a limit on attendees, a waitlist for any beyond that limit, the ability to require an approval for the registration, and more. All of that works pretty well out of the box as well.

## The Cautions

### Everything Else

If you look up CiviCRM you’ll see it offer several components which sound great, like bulk mailing, case management, and volunteer management. They aren’t lying, but they are really at varying degrees of a complete solution. A lot of the components end up a disappointment once you start putting them to heavy use and realize they don’t offer much beyond the minimum. For example, there is a bulk mail tool and some features of it work great, but with some major caveats:

- It’s up to you to configure the server in the best way to avoid being marked as spam, which is hard to do.
- The mailing designer tool is quite limited.
- There is some handling for unsubscribing from lists or opting out entirely, but nothing out of the box that handles scenarios like making sure you comply with Canadian Anti-Spam Legislation.

### Bugs

All software has bugs. The difference between a big corporate cloud service solution and an open-source self-hosted one is that bugs in the big corporate solution will usually be fixed by that big corporation within a day before most people notice.

If there’s a bug in CiviCRM, the best case is that the community has already issued a new release with a fix. In that case, you just need to apply the update. How long that takes depends on your experience and the speed of the server, but could be only around 10 minutes.

The rest of the time, you have to make a decision to either leave the bug alone and accept the consequences or try to fix the code yourself. The latter requires a completely different expertise and there’s always some risk in messing around with code that handles sensitive data.

Most of these bugs are not security risks. Your data is generally safe. The consequences of the bugs are usually more like being unable to edit contributions of time, or issue tax receipts, or view a certain report. They may not be site breaking bugs or security risks, but it’s also hard to fully trust the system housing important data and processes.

### It’s Not Really Free

A common misconception is that open-source means that it is free forever. This is especially pointed out in contrast to the monthly costs of systems like Salesforce or Dynamics (both of which do offer charitable discounts).

CiviCRM is not really free in practice. The code base is free, but that’s it. You still need to spend on:

- A website host, with some heavier processing power and configuration requirements than your typical website
- Maintenance work to provide software updates regularly, either your staff’s time or your money going to a third-party provider
- A consultant to help you set it up properly
- Programmers who can code any changes you need and maintain that code to continue to work with updates

The big question is whether it will still be cheaper for your non-profit. The answer is that it depends. There are several factors to consider, particularly how many users you have and whether the solution can meet your needs out of the box or if you need to add more to it.

For example, a potential best-case scenario: if you’re a charity (not just a non-profit) with less than 10 users and Salesforce can do everything you need, you can get that completely for free. That’s a better much better deal than CiviCRM.

But if you have 50 users, or if Salesforce doesn’t do everything you need and you have to also buy licenses to several add-ons, that’s going to get much more expensive very fast.

### No Email Integration

One of the most central concepts of a CRM system is that it is provides one system of record for everything you need to know about your constituents. Other CRMs like Salesforce and Dynamics provide tools built right into your email to track your email communications into the CRM, so there is no extra work needed to keep those records up to date.

CiviCRM does not do that. The closest it offers is a tool that allows you to cc or bcc a separate email address, which CiviCRM then checks on a schedule (not immediately) and tries to match to a contact in the system. If there is no match, it doesn’t work. If there are multiple matches with the same email, it may put attach it to the wrong one. If you forget to bcc the special email – which in my experience everybody does quickly, no matter how much they liked the sound of the feature – then CiviCRM won’t record it. The result is that CiviCRM is missing most of your email communications, which takes away a huge amount of relevant information and leads to scenarios like trying to sell a client on the same thing that your colleague did last week.

## Conclusion

CiviCRM can be the right choice for your non-profit, but you shouldn’t assume that just because it is designed for non-profits it will be better. Some hints where it may be the best option:

- You already have a Drupal, WordPress, or Joomla site that you want to integrate into, or you want to build a new website on one of those platforms anyway.
- You want a nice simple donation page and/or event management built into your website.
- You don’t need too many code customizations to add features that aren’t provided out of the box. This is particularly true if all you need are the contribution and event components.
- You don’t need to track email communication, i.e. you don’t have multiple people communicating with the client.

If those don’t describe your situation, maybe spend some more time researching and do a full needs analysis before you jump in.
