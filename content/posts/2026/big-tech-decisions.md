---
title: "Decisions on Software Categories"
date: 2026-01-24T17:18:09.000Z
author: Ryan Robinson
description: "Some examples of deliberations in trying to rely less on US Big Tech."
series: "Big Tech Alternatives"
tags:
  - "Tech and Society"
---

In the previous post, [I wrote about the kinds of factors I need to consider in terms of my tech choices](/2026/big-tech-alternatives/). In this one, I'll start working through some of the specific decisions as I try to limit my US Big Tech reliance.

At this stage I have zero connection to Meta or to [X The Everything Including Child Sex Abuse Material App](https://arstechnica.com/tech-policy/2026/01/x-blames-users-for-grok-generated-csam-no-fixes-announced/). I have a little usage of Amazon and Apple, mostly their video streaming services occasionally. I have a lot with Microsoft and with Google. Those last two are the ones where I have some work to do.

There's a lot of possible conversations within this wider conversation, so I'll try to summarize the best I can, one category of tech at a time.

My one last introductory note: I'm only talking about my personal technology decisions, not the ones for work. Those have different variables to consider and are not entirely my decision. Plus this post is already going to be long enough.

## Email

I currently have email with Microsoft. The good news is that I have the old Windows Live Domain offering from when it was free to get email with a custom domain (Google also had this in GSuite). That means I could move the address somewhere else and nobody else should even notice. That part will always be non-negotiable to me: I have to control my address to maintain that freedom.

I have absolutely zero interest in hosting my own email, though. That is a massive can of worms to keep up with things like Gmail not marking you as spam, as well as ongoing costs to keep a server running with zero downtime or you start missing email. I used to have my email in a paid Microsoft 365 Business account and even that I decided was more hassle than it was worth compared to using the free one.

[Proton](https://proton.me/) seems like the most viable other option, at $5 CAD per month for the Mail Plus plan (if paying for a year at a time). They're not Canadian in any way, but at least they're in the EU with much higher privacy standards. They also aren't one the tech giants but their reputation puts them in a similar tier for quality. They are also transitioning to be a non-profit and do not have shareholders who can demand sacrificing quality or privacy in exchange for profit.

That is probably one switch I could make relatively easily on my own. There are a few tradeoffs, including the commitment to the ongoing financial cost. They do have a free version, so that's a nice way to check it out and learn if there are any major problems it would introduce for me. From what I've determined, though, there are a few feature losses I should expect as well, like not including task management and not being able to fully manage a Proton email and an Outlook email in the same app without compromises (e.g. you can add the Proton with IMAP to Outlook but then it doesn't sync contacts or calendar at all).

Proton also offers more intensive plans, with cloud storage, password manager, and VPN. The first two of those I'll also get into more below. VPN may be nice but I don't think I really need it often enough for that to be more than a nice bonus. I would consider the unlimited, but only if moving the entire family and that gets into a much harder challenge than moving only myself.

There's also the related question of which email client I should use, particularly on desktop. If I switched both my individual email and the email shared with my wife to Proton, that's easy: I could just use the Proton apps for desktop and mobile. But if I'm only moving my own, I'm stuck with two less than ideal options:

1. Have separate apps on both desktop and mobile. I'd get full functionality for each, but with the nuisance of maintaining multiple apps.
2. Use Proton with IMAP on desktop Outlook (or maybe Vivaldi), simplifying to only one app there, but with limited functionality.

I'd probably go with the first one realistically, but I'd be a little grumpy about it.

So, I would like to start with just doing my email, then decide if it is worth trying to convert everyone later.

## Cloud Storage and Document Editing

I have a Microsoft 365 Family subscription, shared with some family. That gives the Office desktop apps, OneDrive cloud storage, and a few other useful things like some improvements on the aforementioned email.

In terms of document editing, [LibreOffice](https://www.libreoffice.org/download/download-libreoffice/) would almost certainly handle the simple writing and occasional slide presentation or spreadsheet I need. I could drop using the Microsoft 365 desktop apps and replace it quite easily. I'm not fully confident that everybody else using the family plan would accept that, but it probably wouldn't be that hard for them either if they really wanted to. There may be some more complex business use cases where Microsoft is needed here, but for a lot of us doing personal use, we don't really need it

The main reason we pay for the Family plan, though, is the cloud storage. It was until recently $110 CAD a year for the Family plan that includes 6 TB total of storage; it's now jumped to $145. That 6TB is really 1TB per user, but [you can set up sharing so that the files are distributed across the accounts](/2021/onedrive-family-plan-loophole/).

I have all our photos in OneDrive at full quality, along with many years of documents and website projects and videos and literally everything else. I have a lot of photos with people in them in Google Photos as well, since that makes it easier to find photos by person for things like a digital photo frame or to add a photo to a contact on my phone's address book, but the main place for absolutely everything at full quality is OneDrive with sync to a computer that has a big enough hard drive to hold it all. So, replacing the cloud storage is the hardest part of phasing out Microsoft 365.

[sync.com](https://www.sync.com/) is Canadian and they offer 5TB for $336 per year, more than double the OneDrive price that gives more storage and the other features. Or from the other direction, we could say that the equivalent in price to Microsoft 365 Family would be $6/user/month for the Teams standard that gives 1TB to each user, so at 2 users and 2TB total you've already got the same price as for 6 users and 6TB of OneDrive. That's a big price difference, even if you don't consider the other things that Microsoft 365 Family offers.

Proton's cloud storage offering gives 3TB for the Family of up to 6 users. That's 3 TB total shared, not 3TB per user. That means half the storage - maybe enough for many families including us, but it is significantly less - and the same user count as Microsoft 365, and comes out at $338 per year. That's not an apples to apples comparison, though. That includes their email hosting, password manager, and VPN, so it's certainly a better bargain than sync.com. If all of the family switched all of those components, that's not unreasonable, as it would save costs on more than just Microsoft 365 Family. But do I really want to switch all of that, including from the password manager where we do at least have a great Canadian company right now (see the next section)? And even if I do, am I really going to convince the rest of the family currently relying on Microsoft 365, like the one who I can't even get to join Signal?

I'm not fully closing the door on this one, but the family component does complicate it so I'm not realistically rushing it any time soon.

## Password Manager

This is a rare category where Big Tech doesn't really have that much dominance. Google, Microsoft, and Apple do have some forms of password manager in their products, but they're not as robust as the dedicated offerings. Most of the leaders, if you hear knowledgeable people talk about password managers, are independent. Of course, most of those are still US based.

[1Password](https://1password.com/) is an exception and a clear winner. It's independent, Canadian-owned, often at or near the best of the technology, affordably priced, very user friendly which makes it reasonable to share with family, and has a great security track record. Even if I end up switching to Proton for other things and get the package that includes their password manager, I might stick with 1Password anyway.

In other words, in this category I'm already about the best case scenario there is, so let's move on quickly.

## Search

[I've already written a bit about my experiences testing out Kagi](/2025/types-search/). I like it for good old-fashioned searching for a specific site, and the price is entirely fair, but I realized that's a relatively small portion of what I use search to do. Bing is still so much better for quick answers like checking the score of the Raptors game. AI chat is much better for working out a complicated coding problem. It's really not that often that I want to find a specific site result.

I didn't include DuckDuckGo in those tests. At least for the NBA scores scenario, it seems to be somewhere in between Bing and Kagi. Maybe I should give that a more serious try, but I still most often find myself thinking that it will probably come down to Kagi vs Bing.

This one lands in the feasible, but maybe not worth it, category.

## Browser

There are a lot of browsers to choose from. Most of them are based on Chromium.

I use [Firefox](https://www.firefox.com) on desktop and on my phone, for a bit more privacy and to support a bit of competition in the space. Firefox is working pretty well for me and is the most viable of the "other" players outside of Chrome and Edge.

I know there are some more obscure ones with even better privacy features. [Vivaldi](https://vivaldi.com) may be worth a try and I've heard enough good things, but I don't see this as in the same category of escaping US Big Tech since I'm already on Firefox. I think I'd probably only switch to that if I'm also using its email client, which I would only do if I'm not switching everything to Proton.

## Phone Operating System

It's very hard to use any phone operating system not controlled by either Apple or Google. Of any of the categories here, this is the one most under the firm control of the giants.

I have considered [GrapheneOS](https://grapheneos.org/) on my Pixel phone. From most of what I've been able to read, there isn't a lot of lost functionality, but there is some. This would probably be the biggest privacy win if I pulled it off and was happy about it, but ultimately it is a significant gamble with an extremely important and expensive device. I don't think this is going to be high on the list of priorities.

## Computer Operating System

Our computer uses Windows 11. There's a lot of enshittification, but at least there's also a fair bit of openness to shape it how I want.

I myself would probably be fine using some version of Linux. My wife would almost certainly have some issues, though. I am not going to buy my own separate computer. I could maybe dual boot, but now we're getting into that zone of needing to do more maintenance work over time. It does not seem worth it at this stage.

## Social Media

I haven't used any Meta product in years. [Their involvement in the Rohingya genocide](https://www.amnesty.org/en/latest/news/2022/09/myanmar-facebooks-systems-promoted-violence-against-rohingya-meta-owes-reparations-new-report/) was my last straw, but there were a lot of things before that.

I did use Twitter a lot, until the world's richest crybaby bought it and drove it into the ground to the point that it is now primarily used for extreme right disinformation and for deepfake porn.

LinkedIn is the one big corporate one I use, which is in a different category as a professional network. It still has a lot of the problems of the big corporate networks, but I haven't heard of anything uniquely evil that they've done, and the bottom line is that I probably do need it.

I use ActivityPub federated options, with Mastodon and Pixelfed below in the footer. I have a Loops account but I've been waiting for an app in the Play Store before I really see if I will use it at all.

I also use Bluesky, which is somewhere in the middle of the scale of totally open protocols and greedy corporate influence. I believe some of the developers really do care about building a flexible protocol driven network. They've also got VC funding putting pressure on them to turn a profit at some point, and quickly became more centralized than they wanted to be because everyone leaving Twitter had expectations for moderation. I'd like to see them fully complete their mission to be very open, but I'm willing to invest time there even before they do.

In other words, nothing more to change here.

## Private Chat

I don't use any of the ones with more sketchy security records. I don't even use WhatsApp that seems to have a better security record than the rest of Meta.

[Signal](https://signal.org/) is the gold standard. It is US-based, but at least it is a non-profit and the nature of the end-to-end encryption is that even if an authoritarian government seizes their servers, there is nothing private that is given away.

If somebody isn't on Signal, they get an RCS or at worst SMS message from me, which are less great but at least aren't controlled by any one company. I do also technically have a personal Teams account, Discord, and Google Meet, but don't ever use any of those.

Signal is one of these technologies that I am most evangelistic about because I really do think that it's significantly better and at least as easy as their bigger competitors. It might be the only one of all these discussions where the more ethical option is also at least equal in user experience.

## Video Streaming

YouTube is the dominant force. I do use it, mostly for movie trailers and sports clips. We even have Premium, mostly for the music (more below) but the ad-free videos are also nice.

[Nebula](https://nebula.tv/) is a great option. It's partially owned by the creators who all get paid better per view than they do on YouTube. It's also pretty cheap even if you only watch one or two regular creators in it. The YouTube Premium subscription costs about half as much in a month as the Nebula subscription does in a year, and at least if you don't count the music I spend more time on Nebula.

There are also fully open options like PeerTube on the ActivityPub protocol (like Mastodon, Pixelfed, Loops). I don't know anybody who I would follow that uses it, and I am not creating video myself. Video hosting is also a lot more expensive, so this is one of the toughest ones to simply replicate well without Big Tech money.

Other than the option to cancel Premium, there's not much room in this one for any changes.

## Music Streaming

We have YouTube Premium because that gives ad-free YouTube along with the Music service, and because it's already there on our Android phones and TV and speakers.

I entertained the idea of Tidal, because I remembered it as being artist owned or something. It is now owned by Block (formerly Square), so I don't think that's really any better to support than Google. It's also pretty much the same cost for higher quality music and a more flexible family plan but without the ad-free YouTube videos bonus. About the only way that's a real improvement is if there was other family who wanted to split the cost. Google's family plan is tied to the family management of everything else like credit cards for Play Store purchases, so I wouldn't be able to add extended family to that. Unless that happens, there's not enough benefit from switching.

The closest I can find to a Canadian option is [Stingray](https://www.music.stingray.com), which does not seem great. It has no on-demand music, just channels. Honestly that would probably be fine for me, saving a good chunk of money and supporting a Canadian company, but definitely not for my wife who really likes making her own playlists.

I don't do music streaming nearly as much as my wife does, so a switch here would disproportionately inconvenience her, meaning it is not high on the list of possibilities.

## Podcasts

For this one, I really just want to say: use something that supports any RSS feed. Don't use those like Spotify that have their own shows locked down to only play in their apps, undermining the entire concept of the open protocol podcast.

I personally have been using [Pocket Casts](https://pocketcasts.com/) for years and have always been happy with it. They're owned by Automattic, owner of WordPress, and are US based but at least fairly open and not part of one of the giants. It's been a long time since I experimented with anything else, but that's because Pocket Casts continues to do everything I want it to do.

## Movies

For most people now, watching movies means streaming services. I have some of those, but I definitely don't think they're ideal for a lot of the same reasons that I've been talking about in these posts: constantly increasing costs, too much content to ever consume anyway, and being locked in to paying some US tech giant money every month just to watch your favourite movies. Sometimes you have no choice but streaming to watch something, as they don't even offer any other mode of consumption anymore.

I love physical media. I've got a good quality 4K Blu-ray player. Watching a 4K disc, or even a regular Blu-ray disc with the upscaling, gives you higher quality than streaming, although that gap has closed on a lot of platforms if your Internet can handle it. Physical media also means you own once you pay for it once, not a monthly fee for a service which may not even keep that movie on it forever. I also like being able to look at the shelves. They're collector items for me at this point.

There is also the in-between option of the digital purchases or rentals from the Big Tech stores: Google, Amazon, Apple, and formerly Microsoft (they won't sell anything new but you can still watch if you bought anything before). Referring to a "purchase" on these is misleading, as what you are really purchasing is an indefinite license for as long as the store continues to operate. There's a lot to be said for the convenience, and there can be good sale prices, but ultimately you're handing money to Big Tech again to maybe continue to have access as long as you don't leave their platform. When Movies Anywhere was first announced, I was optimistic that would enable the ease of buying movies digitally but also still freedom to move between platforms. Unfortunately, that never expanded beyond the US or to include some of the smaller distributors.

## TV Operating System

Any TV that has apps is going to be doing a ridiculous amount of data mining. It is still possible to buy TVs without an Internet connection, but of course that means you don't have easy access to the apps you want, so then you buy the extra box that does all the data mining and is less friendly integration.

Anything with Roku the worst for data collection and ads, while having significantly worse app support in Canada and mostly being only on low to mid-range TVs. They get some credit for not being owned by one of the giants, but that's it. Samsung doesn't support Dolby Vision at all, so I ruled them out, too. We considered the higher end of the TCL mini-LED to save a little money, but from everything I read, OLED makes more sense in a basement because of the deeper blacks and would be worth the extra (mini-LED is great in more sunny spaces, though). LG could have worked with their revamped webOS system on it, and all indications are that they are great as well, but ultimately we decided that if we're going to have some massive data mining anyway, we may as well have the best apps and the most integration to all our other Google things and go with a Sony OLED that has Google TV on it.

So, in terms of this one, the only real consideration is whether I should make more effort to block the data mining. I could set up [a Pi Hole](https://pi-hole.net/). I believe that's relatively cheap and simple to set up.

## Smart Home

We bought a house not long after smart home devices started becoming a thing. We immediately needed a new thermostat, so we got an [ecobee](https://www.ecobee.com), a Canadian company with a great reputation so far. That continues to be my favourite smart home thing. We added some [Hue bulbs](https://www.philips-hue.com/en-ca), which are nice but expensive and not as easy to use as I wish they were. We have tried some smart plugs, including the miserable to set up Belkin Wemo that are now no longer supported, which are not really that much more useful than having a power bar with a timer.

The closest thing we have to a hub for managing all of this is Google Home, which has been adequate for my minimal attempts to use it. There are some little things like having a "good morning" routine which are nice. But it's not that powerful, and obviously we are sending all the data off to Google for every trivial request, which is less than ideal.

I would like to migrate to [Home Assistant](https://www.home-assistant.io/). It's not that expensive to get some hardware and then it all runs locally. It's all open source, with a strong community so even if the organization providing some oversight fails, the technology would likely survive. From what I've heard, it's a bit of a pain to set up, but once it is, there isn't that much extra maintenance to keep it going. This seems like the kind of project I might actually enjoy playing with, though, and it doesn't have much risk of breaking anything I really care about the way that GrapheneOS feels like such a risk to try on my phone.

## Git Repository

I use [GitLab](https://about.gitlab.com/) at work, on a hosted instance. Because of that, certain code I write personally I also put in [my own GitLab.com account](https://gitlab.com/ryan-l-robinson), because that's where I know my way around the CI/CD functionality already.

Other than that, I currently have the rest of my personal code in [GitHub](https://github.com/ryan-l-robinson), including this website. That's owned by Microsoft, but operated mostly independently. I don't have any real problems with GitHub. I would definitely need to keep a GitHub account either way, as it is how to interact with a lot of other projects. But ultimately, yeah, it's a US Big Tech, and I'd rather something else if I could.

There are other options, like the Canadian [Worktree](https://about.worktree.ca/) and the European non-profit [Codeberg](https://codeberg.org/). Worktree gets an edge as Canadian. Codeberg gets an edge as non-profit and more established. I'm going to try Worktree first because it is nice to see a new Canadian company trying to do this. We'll see how it goes from there.

This could still go a few different directions. At minimum, some things will be in GitLab. Some other things will probably be in Worktree and/or Codeberg and/or GitHub.

## "AI" Chat

This is still a fairly open area, although it isn't a coincidence that a lot of the most popular ones are also associated with the Big Tech companies working with massive sets of data. OpenAI and Anthropic are two that are big now and came out of nowhere as technically independent, but they get a lot of their funding and cloud services from Microsoft and Amazon respectively, so they aren't quite Big Tech but they are Big Tech adjacent.

There are models that can be run locally, but you need a computer with enough power and some technical expertise. Considering I essentially only use it for code, my ideal probably really is to have a model that is highly specialized for Drupal development and run it locally. That would completely keep the big tech companies out, with no subscription fees and no data being sent off to them. I haven't come across anything that is even close to ready for that yet.

In the meantime, there are some smaller companies that seem to have more ethical standards, like [Nous Research](https://nousresearch.com/). I have not tried them at this point. If I do get to a point where I feel like I need to pay a US company, I might on principle try that first and see if it can be maybe not as good but at least good enough. Most likely, I won't be personally paying for any AI.

## Conclusions

So, what of those are the most reasonable? I think I've mostly come down to these ones that I can mostly do without inconveniencing anyone else:

1. Move code repositories to WorkTree and/or Codeberg and limit GitHub
2. Set up Home Assistant and phase out Google Home
3. Move email to Proton instead of Microsoft
4. Set up a Pi Hole for network-wide ad blocking including the smart TV
5. Switch to Kagi for search instead of Bing

What does that leave me? For Microsoft, that would get me down to:

- Windows
- LinkedIn
- Xbox
- OneDrive

And for Google, I'd still have:

- Android on my phone
- Android on the TV (but somewhat limited in its data collection with the raspberry pi)
- Photos mostly to populate the photo frame
- some YouTube, including Music
- occasional movie rentals or purchases

That's still a fair bit, and there might be a second wave of more dramatic changes I could make after this first group to cut it down even more, but I do think even if I only pull off those 5 changes that is a worthwhile effort.
