---
title: Twitter, Mastodon, and CounterSocial
date: 2022-11-21T13:18:09.000Z
author: Ryan Robinson
description: "Discussing some advantages and disadvantages of current microblogging options."
tags:
    - Tech and Society
---

If you're on this site, you're probably aware enough of the chaos around Twitter. I started to write a post about **why** I was leaving Twitter, narrowing in on the various terrible management practices (printing off code, firing based on lines committed, losing entire security teams, etc.) at the time when everybody was focussed on the verification disaster. Then all the stuff I was worried about completely exploded so now everybody is well aware of that, too, and it is likely a matter of days to weeks before Twitter completely fails. So there didn't seem to be much point there.

I also started a post about **how** I left Twitter. I was going to detail how I repeatedly notified everybody there while I started moving more and more to using Mastodon and CounterSocial instead. Then after the latest round of Twitter employees being fired, I realized about 7% of my Twitter follows were already on Mastodon. I'm not sure that many people really need to hear about how I migrated as if that was unusual.

So, how about this post idea instead: a quick rundown of Mastodon and CounterSocial and how they compare to Twitter. For the most part, Mastodon and CounterSocial line up with each other, as CounterSocial forked off of Mastodon in the past over some licensing disagreements (I have no interest in picking a side), but there are a couple big differences.

## The Algorithm

The biggest difference from Twitter to the others is that Twitter relies heavily on the algorithm to determine what gets on your feed. Their business model - like all the other major social networks - is based on selling as many of the most targeted ads as possible. You are the product and advertisers are the customers. To do that, they have to collect as much data about you as possible. To do that, they need to keep you engaged as much as possible. To do that, they develop algorithms to target content to you, regardless of whether you actually chose to see that content, so that you can never be "caught up" and return to your normal life without fear of missing out.

Much has been said about the dangers of these kinds of algorithms. They prey on some of our worst impulses, feeding us things that make us angry. Many right-wing personalities have caught on to this, realizing they can make a big following by gaming the algorithm: they say something that gets people mad, everybody shares it either in support or to dunk on it, and all of that adds up to exponential growth in their audience as the algorithm boosts it even more. The end result is more hate speech, more anger, and more polarization.

They also need to collect massive amounts of data in order to categorize us effectively for advertisers. While Twitter had a pretty good security record, this does open up the potential for huge data leaks. I don't expect that pretty good record to last much longer, which is why I'm inclined to delete my Twitter instead of merely deactivating or stopping use of it.

There certainly are some positives as well. They are designed to give you what you will respond to, so sometimes that's really good, with it helping surface people you want to follow who may not be big enough to trend in general but is exactly the kind of person you want to know about (especially when dealing with marginalized groups), or making you aware of breaking news very quickly.

There's also the simple reality that many people are happy with this trade, giving up loads of data rather than needing to pay for a service. It lowers the barrier to entry.

Mastodon and CounterSocial do not use an algorithm. You see what you chose to see. You can follow accounts and hashtags and those can boost (retweet) from other accounts into your home timeline. You can also check in on your local timeline (both Mastodon and CounterSocial) or global federated timeline (Mastodon only, since CounterSocial does not federate with anybody else). But you'll never get recommendations decided by a mysterious machine learning algorithm. The speed of discovery is limited to human speed, not machine speed.

## Verification

Verification - at least before the recent changes - was the largest advantage Twitter had over Mastodon and CounterSocial. Trust is important in an information ecosystem to know that people are who they say they are.

Mastodon and CounterSocial do have a form of verification, but it's a verification of links, not of identity. It is a system that allows you to set up a link on your website to Mastodon/CoSo, which confirms that the website owner and the account owner are the same person. It does not necessarily confirm that the account is who they say they are. Somebody could impersonate me by setting up a fake website to go alongside the fake account. That it is a lot more work, so it might happen in targeted ways of public figures, but it won't happen as much in casual mass bot attacks. It also doesn't give a "blue check" equivalent, so it isn't obvious at a glance whether somebody is who they say they are. As more public figures and journalists move to Mastodon and to a lesser extent CounterSocial, this will be really interesting to watch whether they need to come up with some kind of system, and in the case of Mastodon, whether that would even be possible in a decentralized system.

## Servers and Onboarding

Here's where CounterSocial aligns with Twitter instead of Mastodon. For each of those, you sign up and you're in. It's pretty straightforward, not much different than any other website.

Mastodon gets more complicated. It's a protocol of federated servers called instances, not a "platform." This means upfront that you have a question to figure out: what instance should I join?

Veterans will think this is no big deal, but if you've never dealt with something like this before, staring down hundreds of servers is not an obvious choice. There isn't much in the way of advice for how to decide, or tools to filter a list. Do you care about the data residency of the server? Do you care about how the admins make decisions? Do you care about what rules it has, or what other instances it has blocked? Do you want general topic or a specific interest or identity group?

I landed on the largest Canadian server, mstdn.ca, but only after trying four others over four days. A couple were tech specific. A couple were big general ones. Once I found this one, I was happy for Canadian data residency, general topics, an admin who has so far done pretty well keeping up with the demand, and reasonable rules.

If you pick a bad server, you may end up with some problems. Maybe that server doesn't moderate well and there's hate speech (or spam, or porn). This means you get a lot of hate speech in your local feed, but it also means other instances might block your instance so that the hate speech doesn't spread. That leaves you isolated to your server, completely losing out on the advantages of the whole federated concept.

You could end up spending a couple hours getting set up only to have a miserable experience on a bad server and conclude Mastodon is horrible, when really that one server was horrible. In my opinion it's worth it in the long run, but it's a bit of a mess to sort out.

## User Interface

Twitter has polished their interface over the years. There may be things you don't like, but they know what they're doing.

Mastodon depends wildly on which app you use. I'm on Tusky on Android, which is pretty good but does have some weird holes like searching for users on other instances without entering the whole instance address. But it is attractive, easy to use, and has a good range of options.

CounterSocial is the definite loser here. It isn't hard to use but it doesn't fit the Android design language at all, it only has a pseudo dark mode - no light mode and no true dark mode - and is weirdly small on the screen.

## Features

A couple significant features are not in Mastodon or CounterSocial that you might have gotten used to on Twitter.

There is no quote tweet. This is intentional as it is a feature that is most often used for piling on harassment. But there are lots of positive uses for it as well, so the loss will be felt by many. I used it a lot to comment on a news story as I spread it onto my feed.

Search on Twitter indexes everything, all the text of every tweet. Mastodon and CounterSocial do not. They only index profiles, hashtags, and posts that you have already interacted with. This is intentional as it gives those posting some control: if you want it to be searchable, add the hashtags you want it to be searchable by, but if you want to be able to keep it a little more private, you can. This limits harassment of mass searching for text and attacking anybody you find.

There are no circles, although if you were intense enough you could replicate a less flexible version of it by making your own instance and recruiting all your closest friends to it. You can choose to share only to your followers, only to your instance, or available to be federated globally, so that is really nice. If your instance was effectively your circle, problem solved, but that's not something that most people would achieve.

Mastodon allows you create bots, but you can flag those bots as bots and it will be obvious when viewing. This is great for unofficial bots like news organizations to know when this account is not being actively monitored.

The latest version of Mastodon allows editing your posts. This isn't in the Tusky app on Android that I use, so I cannot comment on it. Prior to this, both Mastodon and CounterSocial had a "delete and re-draft" which was also better than nothing.

Content warnings or content wrappers are available. The consensus at least for Mastodon (not as much CounterSocial) is to use these quite liberally. They aren't only for potentially sensitive content but are really any kind of subject header for the post that allows people to decide whether they want to read more or not. This is possibly my favourite feature and I've used it for anything from Ontario politics to talking about Twitter to spoilers of Black Panther Wakanda Forever.

Mastodon and CounterSocial can schedule posts in advance, which is nice. Twitter could to that with some third party tools but not in the official apps.

Mastodon and CounterSocial both allow you to auto-delete posts after a certain period of time. This is also great if like me you appreciate when it feels more like an ephemeral conversation than a permanent archive of everything I ever said.

Mastodon and CounterSocial allow you to mute somebody for a shorter period of time, not always an all or nothing permanent decision. This is great if you just need to tune out somebody for a bit but you're happy to see them again later.

Many other features are comparable, including muting, blocking, and adding image descriptions for accessibility.

Overall, in raw features, Mastodon wins out. Losing circles is unfortunate, but some of these other tools truly are great to have and easy to use.

## Payment

As discussed above, Twitter is mostly funded by mining data for targeted ads. They also added Twitter Blue as a freemium style plan to get more features, which the Elon Musk era is leaning into.

CounterSocial is freemium. You can do all the basics for free, but you'll need a Pro account to do several things including some of the features I've named above.

Mastodon is donation based. The instance admins pay the costs and hope that they get enough donations to cover it. So if you are joining Mastodon, have found a good instance, and have some room in the budget, consider kicking a few dollars to your admin.

## Culture

Twitter had a famously unique culture. It was the place where news broke. It was the place where movements started and spread to the mainstream, including both movements for justice and movements of hate. There were plenty of niche corners for your favourite topics, but you would also inevitably get caught up in the big news of the day.

It's hard to completely judge Mastodon and CounterSocial since they have been overrun by Twitter exiles. But what I can mostly say is this:

CounterSocial seems to be much more the home for those who want to talk US left-wing politics. That's a good portion of the firehose. As a Canadian, that isn't a great selling point to me. Mastodon certainly has those corners, but it's also easier to avoid them and to fully delve in completely different topics instead, especially technology.

Both do suffer from a bit of arrogance. They're proud of the fact they built these networks without big money algorithms, and they should be. But sometimes they also come across as looking down on those who are new and confused why things are different. I've seen this more with CounterSocial which loves to set itself in opposition to Mastodon as well as in opposition to Twitter, but I have seen it in Mastodon as well.

Both have stronger stances on paper against hate speech and trolls than Twitter. But moderation is a lot easier said than done. Ultimately this all depends on moderator abilities to keep up and to act consistently to avoid frustrations. It will be interesting to see what happens here as Mastodon in particular is growing so rapidly and **will** become a target.

CounterSocial is a bit more "extroverted" by which I mean that it is more normal to get replies from (mostly friendly) strangers.

## Conclusion

I have just completed deactivating my Twitter.

I am really finding a new online home on Mastodon. It's where the most people are headed now and has a great range of features, topics discussed, and it has a friendly interface. It's really just the hurdle of picking an instance you'll need to get over first, and that is understandably a larger hurdle than is worth it for some people.

CounterSocial will be a great option for a lot of people. It's a lot faster to get an accurate gauge of whether it's good for you or not, without the instance decisions, so I would say to go ahead and check it out if everything I've written makes it sound appealing to you. It may not be my long-term option, aside from the fact that a few people I know are on there and I would like to keep up with them.
