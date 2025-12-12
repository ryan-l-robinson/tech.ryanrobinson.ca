---
title: "Accessibility: Level of Descriptiveness"
date: 2023-06-11T13:46:00-05:00
author: Ryan Robinson
description: "An attempt to navigate the tensions between a lot of detail and more efficient browsing in accessible text alternatives."
tags:
  - Accessibility
---

There's a tension in discussing accessibility for screen reader users that comes up sometimes. Most recently and perhaps most commonly is about ALT text, but it can also apply to other things, too. The main conflict is between:

> You should provide lots of detail in order to make sure a blind user has access to as much detail as a sighted user can. If you've got a picture of a cute cat, you provide all the details about the cat: colour, size, facial expression, posture in the shot, anything that a sighted user would be able to see.

Or:

> You should provide only the core details so that the screen reader user can get to the important point as fast as possible.

It might be tempting, especially if you're in the first camp, to frame this as something like equality vs efficiency. But it's really two different definitions of equality: is it equality in detail or equality in function.

Another way to think about it is in terms of cognitive processing time. When an image has more details, it doesn't really add to the time that a sighted user needs to process it all, unless it's a Where's Waldo kind of scenario that is intentionally hiding details in a way that is not available at a glance.

On the other hand, adding more text description that needs to be spoken out loud is a linear function. If you have twice as much descriptive content, that's twice as much time to listen to it. Maybe you structure it in such a way that they can get the basics out of the way and then have the longer description in a different spot that they can easily skip over, giving them a choice, but that's not always going to be easy.

So what's the answer? How much description should be added for screenreader users? Like many things in life, I think the answer is "it depends."

Here's one scenario: you're posting on Mastodon about how cute your cat is. In that kind of scenario, the point is the details. You probably should have a description of the cat and what makes it cute. If somebody is choosing to listen to the description of the image attached to a post about how cute your cat is, it's probably fair to assume that they want the details even if that takes a minute or two.

A chart of data may vary on the context. In a quick casual social media post, it's probably better to get to the point. Imagine a chart that shows the increase of CO2 emissions over time, spanning several years with lots of data points, but the clear purpose of the chart is to show how badly things are increased. Somebody probably doesn't want to be scrolling social media and get bogged down in a 5 minute narration of data points naming every year plus exact numbers for that year that all blend together and are hard to interpret. That's probably not what they're there for.

But what if it's the same chart but as an academic resource? If somebody is doing research, they might need to know the exact data points, so the ALT text (or corresponding text nearby) needs to be quite detailed.

Side note: I once heard about efforts to aid screen reader users encountering a graph to get the point better than a list of numbers, that relies on sounds. If the data is going up, it makes a higher pitch sound. If the data is going down, it makes a lower pitch sound. It will be interesting to see if something like that ever becomes standardized enough to work.

This exact same distinction can also apply to other context like menu items: do you want to add extensive aria-labels to force verbose experiences on blind users? The title attribute is a nice balance in that example. At least in Jaws, there is a user setting for whether to read out title text. This allows developers to empower the blind users to decide for themselves whether they want the verbose description or not. And it's always better to empower users to have control over their own experience whenever possible. Provide title attributes and if they want it, they'll have it; if they don't want it, it won't slow them down.

In any case, the main point is that ALT text and accessibility in general is not always an obvious formula. Sometimes there are tensions to navigate. The most important part is that you are thinking about it, and when possible listening to the real experiences of users to steadily improve how to navigate those tensions.
