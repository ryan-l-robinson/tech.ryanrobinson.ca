---
title: 'Microsoft Teams: Phone Numbers'
date: '2021-08-19T02:58:00-04:00'
author: 'Ryan Robinson'
series: MS-700
tags:
    - "Microsoft 365"
---

I personally have not used Microsoft Teams as a phone system. My employers have stuck completely with Internet-based communications, mostly to others in the same organization. But if you want to integrate your organization’s phone system into Teams and have your phone calls ring through in Teams, that’s absolutely possible.

Here’s an introduction on some of the things to consider:

## Calling Plan or Direct Routing

Your first decision in setting up a phone system in Teams is between:

- Calling plan: simplest solution, letting Microsoft handle everything
- Direct routing: allows for more complexity and keeping a previous phone provider

## Add Phone Numbers

There are two basic options when you want to add phone numbers to your Teams deployment: add new numbers, or port existing numbers from a previous provider. You can do this from the [Teams Admin Centre](https://admin.teams.microsoft.com) -&gt; Voice -&gt; Phone Numbers.

Ordering numbers has a few types to choose from:

- User: a regular number for a single user
- Dedicated conference bridge (toll or toll-free): for conference rooms with multiple people calling in at once
- [Call queue](https://docs.microsoft.com/en-us/microsoftteams/create-a-phone-system-call-queue) (toll or toll-free): used to define how to route calls between multiple agents, with features like wait music
- [Auto attendant](https://docs.microsoft.com/en-us/microsoftteams/create-a-phone-system-auto-attendant) (toll or toll-free): attendant for callers to be able to select how they would like to be routed, e.g. select 1 for tech support, 2 for accounts, etc.

Along with the type, you’ll need to specify a location – country and area code – as well as the quantity for how many you need.

## Emergency Addresses

Every phone number has to have an emergency address tied to it, so that if a user calls emergency services they can trace it back and find them. With Teams phone systems, you can and should have one emergency address configured for each of your office locations. When you are assigning the number, you then specify at which emergency location that person is working.

## Dial Plans

If you’ve worked in most large offices, you’ve probably encountered the use of something like “dial 9 for external numbers.” These kinds of scenarios are handled with dial plans. You can specify what digits (up to 4 digits) must be used to dial out to external numbers.
