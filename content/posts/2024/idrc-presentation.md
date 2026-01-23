---
title: IDRC Presentation on Open Menu
date: 2024-02-21T23:22:36.000Z
author: Ryan Robinson
description: "I recently presented to the Inclusive Design Research Centre about work on an open menu concept. Here are some of my key takeaways."
tags:
  - Drupal
  - Accessibility
---

I recently joined the Inclusive Design Research Centre to present along with a couple of colleagues how we designed an "open menu", a header-focused main menu navigation that works much better for screen reader users and also provides some other benefits like a Ctrl+F find.

[The video is available on the IDRC wiki.](https://fluidproject.atlassian.net/wiki/spaces/fluid/pages/11513504/Community+workshops+and+design+crits)

I don't have a transcription of the entire presentation, but here are some of my key notes from the perspective of the Drupal developer.

## The Process

The co-design process loop often went something like: Ashley (accessibility consultant) described what worked and what didn't about her experience as a screen reader user, Mark (digital experiences librarian) made an HTML mockup of what we should try instead, and I figured out how to make Drupal do that in the best and most sustainable way. Repeat as necessary until we're all happy with the results.

I had to consider several issues, not only accessibility. For example:

- Timeline: One bit of important context was the launch timeline. I had just been hired and I was learning my way around the old site at the same time I was beginning the new site. We had a deadline for the new site to launch, what was at the time expected to be the Drupal 7 end of life (it got extended again after that). These menu considerations came as part of this rebuild process. That meant there was some balancing between what is ideal and what can be done in time.
- Security: Failure to meet that timeline would have security implications if we didn't meet it. We could have launched the open menu after launching the new site, in order to hit the end of life deadlines, but it would have been awkward at best and required restructuring the main visual menu at worst.
- Visuals: While not the main focus of this feature, there is visual design for sighted users to consider. We want it to still be attractive and easy to use for sighted users as long as it doesn't come at the cost of screen reader users.
- Maintainability: How do we ensure we keep this working and accurate? A major important point to me was that there needed to be one source of information that fed both the collapsed menu and the open menu. Otherwise, there's a high likelihood that at some point the two will get out of sync with each other, where somebody adds to or rearranges the collapsible menu without considering the open menu or vice versa. Sometimes an inaccurate accessibility feature is worse than no accessibility feature at all, because people wouldn't think to go try the less-ideal collapsible menu if they've been assured they have all the same content.
- Open source models: What can be done in Drupal core, what in a contributed theme, what in a contributed module, what in custom code? If a module is close to what we need but not quite there, how do we navigate what we report and try to get the maintainer to change vs what we should rebuild ourselves?

## Lessons

This was a fascinating project for me in ways much deeper than the technical details. So here's a few things that stood out to me:

- Empathy vs checklist: The biggest lesson is that personal experiences of disabled people really matter. I'd worked with accessibility before, but mostly at the level of a WCAG (Web Content Accessibility Guidelines) checklist in a contract consultancy context where finishing the job under the budget was the main priority. This whole project was a great example of taking the time to get it truly right to real user experiences. Ashley explained that even though a standard Drupal menu passes the checklist, it can be really time-consuming for her. Hire someone if you can to fill that same role. It opened my eyes to a more emphathetic approach rather than a checklist approach. Maybe you can't hire somebody - most of us can't, and even our time with Ashley was limited - but at least take the time to sit and try to imagine what it would feel like to do this without sight, or without hearing, or without a mouse. Don't just hit the WAVE extension button in your browser and declare it done.
- Multiple solutions: Sometimes there isn't one solution that is perfect for everybody. We tried to figure out a way to make a menu that was optimal for all users. But the reality is that the standard collapsed navigation is ideal, tuned over decades of web development, for most sighted users who can skim at a glance without it taking much cognitive load. So we took an additive approach instead, keeping both versions of the menu. In hindsight this isn't really that shocking - we do the same for things like having a video and a transcript - but I hadn't really thought about it for something as central as the main navigation.
- Jargon: Getting the descriptions for each page was more challenging than expected and one of the reasons why were questions about jargon. Often in talking about accessibility, we talk about wanting the most generic phrasing that everybody understands, avoiding jargon. That's great for new users not familiar with the jargon. But there is a tradeoff for those who are familiar with it: that's probably what they're looking for. If they're looking for the jargon and it isn't in the description, it doesn't show up from a site search or in the open menu items page where a Ctrl+F might find it. Some of that could be mitigated by including the jargon and the broader definition, but that has [a tradeoff of making the content longer to sift through, which is especially more time consuming on a screen reader](/websites/accessibility-descriptive/). Making it more accessible to one group can make it less accessible for others.

## Coming Soon

I have another post coming about how this was technically achieved, which was relatively straightforward. The bigger lessons were here, in the process, but I do want to share how it was done for those that are interested in replicating it.
