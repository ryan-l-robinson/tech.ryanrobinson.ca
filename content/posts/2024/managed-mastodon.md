---
title: "A Brief Experiment in a Mastodon Self-Managed Instance"
date: 2024-03-25T00:16:00.000Z
author: Ryan Robinson
description: "I did a cheap trial of managing my own solo Mastodon instance. I learned a few things about its signup, limited federation, domains, markdown, and storage."
tags:
  - Social Media
---

I took advantage of a Black Friday deal from [fedihost.co](https://fedihost.co/) to try out using a managed host for a Mastodon account, as compared to the large Canadian instance [mstdn.ca](https://mstdn.ca) that I have been using since my early days of joining Mastodon. It was $1/month for the lowest plan for a year, which is pretty solidly in impulse purchase territory, so it was a worthwhile idea.

To clarify, this is not an experiment in self-hosting in the strictest sense. I'm not doing any of the server or software maintenance myself. It's more like self-administering. Some professionals I'm paying did all the server and software work. I just administered users, which in this case was just me, and general settings.

Anyway, here's how that experiment went. You might already have some ideas from words I've used like "brief" and how a Black Friday deal experiment is now all past tense.

## Signup

The process with fedihost was easy. It gave me the DNS record to create first, then I came back the next day to confirm it propagated, and signed up including a payment method. Soon after, I got an email that everything was ready, with a link to the dashboard. That dashboard is minimal but had a few options for managing services like how many days of cache to keep, which would quickly become an essential option (see under Storage below). It also showed me a randomly generated admin password I could use to sign in.

## Domain

The biggest benefit of this option may be that I could use a subdomain of my main domain, creating mstdn.ryanrobinson.ca. If you really care about your branding, that's a big selling point, plus as Bluesky has demonstrated, it's a form of verification that you are the same person who owns that website. Mastodon does have a more subtle version already, where you can put a link on your site coded in the correct way, then put that website URL in your profile and it will show with a check mark. But that's fairly hidden on a user profile, not directly in a handle or anywhere close to the now-infamous blue check on Twitter.

The downside of this, at least in my case, was that now my handle became much more unwieldy. I have a pretty great handle at mstdn.ca, simply @Ryan@mstdn.ca. That's rare with my very common name to get that good of a handle. It's clean, easy, not taking up much space. With the managed hosting, I got @Ryan@mstdn.ryanrobinson.ca, which gets to use my domain but looks much clunkier.

## Trending

If I had thought it through, I would have predicted that trending would be less useful. With less wide range of federation, less tags and links will come in.

What I never would have thought of was that I would need to approve each tag and each link before it can even show up within the typical user app. I get why that's an admin moderation function, to not let hate speech or whatever else you don't want to be visible for other users. But when you're the only user, it means you can't see anything trending in the app until you've first already seen it in the admin panel, for which I get a few emails a day to review. Those emails will decrease quickly because once I've approved it once, I don't need to again, but that is an annoying extra step with zero benefit when it's a single-user instance.

## Search

Search is also less powerful with less federation, although that was less of a problem in my few days of testing. It's not that often that I really want to search for a topic. At this point I mostly follow who I follow, not going looking for what strangers are saying about something.

## Moderation

Moderation is the area I can probably say with least confidence because I was doing it for such a short period of time. I didn't have a lot of connections, and I did not end up doing any of my own moderation. That's the upside of the limited federation. This was the thing that I was worried would be the biggest extra piece of work, because I really have no desire to squash a bunch of crypto scammers or hate speech on my own. Maybe it would have become more of a problem on a longer scale.

## Markdown

This managed hosting instance did not support markdown as mstdn.ca does. This isn't a huge deal, but I do appreciate having it, especially for links since there isn't a guarantee that every app has link previews so I want to at least give it readable link text.

## Local Feed

I rarely use the one on mstdn.ca, but the entire concept of a local feed is meaningless on a single-user instance. It's just me. I may as well look at my profile.

## Storage

The cheap plan, the one that was so cheap on Black Friday as to spark an impulse purchase, comes with 10GB. That feels like it should be a lot for one social media account with auto-deleting posts.

After initial install I was using about 2.5GB.

By the second day, I had a warning email letting me know that I was over 80% and if I fill it up, I will be automatically upgraded to the next plan which does not have the Black Friday bargain prices. So I went in to the dashboard and cut the caching option from keeping 10 days down to only 1. The next morning I saw I was back down to about 65% used.

I still got another warning email the next day.

Today I decided that there was nothing left to learn in this experiment and decided to delete. I was at 97% of my storage even though I had barely logged in today and had the 1 day cache.

It was inevitable I would have been paying more for it soon, which takes it not only past the $1/month bargain but also past the $6/month regular price for that plan that is roughly equivalent to what I am donating to mstdn.ca.

## Conclusion

Overall, it's a fine option. It's reliable, affordable, not a lot of extra work, and has some benefits if you really want to be able to shape it to be exactly what you want from moderation decisions to the domain.

But I think it's a step backward in my case. It's still some more time required, a clunky handle, and almost certainly would have ended up paying a fair bit more to support more storage than I am donating to mstdn.ca. The storage challenge helped rush the decision, but really, it was already becoming clear that there was nothing else left I might learn to convince me it would be better off than what I'm using now. I'm going back to mstdn.ca full time.
