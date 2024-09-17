---
title: 'Microsoft Viva Topics: Configuration'
description: 'An introduction to Microsoft Viva Topics and how to configure it.'
date: 2021-02-24
author: 'Ryan Robinson'
tags:
    - Security
    - Microsoft 365
---

Microsoft recently launched [their new Viva platform](https://www.microsoft.com/en-us/microsoft-viva). Nothing about the announcement was radically new – everything had at least been previewed – but the new Viva branding helps tie them together with the shared goal of improving the employee experience.

The most interesting component for me is one piece that did launch that day: Topics. In this post and the next I’ll break down my first impressions from some simple tests.

## Configuring

The announcement blogs about Viva Topics did not make it clear how to get started. It also isn’t obvious; unlike some of the larger workloads in Microsoft 365, there is no new Viva Admin Centre. [Fortunately it is clear in documentation elsewhere](https://docs.microsoft.com/en-us/microsoft-365/knowledge/set-up-topic-experiences) where to get started in the main Admin Centre.

Once you find the process, it’s straightforward with some useful options, such as:

- **SharePoint sites**: you can specify to look for topics on all sites, all sites except those specified, only those specified, or not at all. In most cases, all sites will make the most sense. Don’t worry about permissions as it already utilizes the Microsoft Graph to make sure that nobody sees content they shouldn’t.
- **Exclude topics**: if you know some topics that you don’t want in advance, you can upload a csv file with those. You’ll always have manual control over topics, though, before they get published. If you just let it find everything it can and then remove them from there, that’s also fine.
- **Who can see topics:** you can allow the topics functionality for all users, only selected users, or nobody. This feels like a redundant setting to me as there is also an extra license needed to see topics.
- **Who can manage topics**: you can allow editing topics as well as adding new topics to different permission groups. You can leave this open for everybody, but in this case most larger organizations will want to restrict it to a handful of dedicated admins determined by a security group.
- **Topic centre**: name and pick the URL for your new topic centre, a special SharePoint site. You may want this to be generic like “Topic Centre” or you may want something more specific if you are going to use it for a more precise purpose like a client listing.

When you’ve picked all the desired settings, confirm that you’re ready and the features will be enabled. Note that it took an hour or two after completing this setup before the topic centre was ready for my use. That makes sense, but it doesn’t warn about that delay; if you are confused why it doesn’t seem to be working right away, you likely just need to wait.

![](/assets/img/2021/02/topics-config-screen.png)
_Topics configuration settings, after the initial wizard_

## The Topic Centre

The Topic Centre which is created after the setup process above looks like a typical SharePoint site, but with some specific page types. The homepage shows topics related to you. The Manage Topics page is the core feature for administering topics on an ongoing basis. This page shows all of your organization’s topics, broken down by the stage of approval:

- **Suggested**: topics found by the automatic search for you to review. The question to move from this stage is whether you do want that to appear as a topic across your organization. If not, you can remove it. If yes, you can confirm it.
- **Confirmed**: topics which have been confirmed as a real topic of interest for your organization. For items here, take the time to view the page and make some manual edits to the topic before publishing.
- **Published**: topics that are active within your organization and can be seen by typical users with the appropriate license and settings.
- **Removed**: topics that have been removed from your organization.

![Sample of topic centre](/assets/img/2021/02/topic-manager.png)

This is the hub for your Topics admins to keep things organized. I found it clean and intuitive to use. I imagine in large organizations that there will need to be somebody regularly checking in on the generated topics and keeping things organized, but the tool to do that is straightforward enough that the technology won’t get in the way.

Overall, it’s off to a great start. In the next post, I’ll talk about what this looks like for typical users seeing the topics within their workflows.
