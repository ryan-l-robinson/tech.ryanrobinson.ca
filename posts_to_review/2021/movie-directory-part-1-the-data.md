---
title: "Movie Directory Part 1: The Data"
date: "2021-06-18T09:52:00-04:00"
author: "Ryan Robinson"
tags:
  - "Microsoft 365"
  - "Movie Directory"
---

I recently decided to do more experimentation with Dataverse (formerly Common Data Service) in the Power Platform. I did this by making myself an app to solve a need I have myself: a movie directory app. I wanted to be able to:

- Track in what formats I own a movie (Blu-ray, DVD, digital)
- Link to JustWatch to find where I could stream a movie
- Assign a movie to different lists
- Streamline a process for grabbing the movie cover image

There are good apps out there that do these things, but I never found something to do them all in one place. More than intending to use it, though, it was mostly an excuse to try out some things in Dataverse and Power Apps. In this post I’ll describe my data structure in Dataverse. In the next post I’ll walk through a couple complicated features of the Power App itself. Then I’ll detail the Power Automate Flow that I used to help streamline grabbing movie cover images, with some limitations on how much I could do without a premium license.

Here’s the table that I created, appropriately named “Movie.”

![](/assets/img/2021/06/Dataverse-table.png)
_Table summarizing my Movie DataVerse structure_

Many of these fields are the defaults that come with any new table, but this is what I added:

- **Cover**: data type Image and the flag set for Primary Image.
- **JustWatch**: data type URL.
- **Last Watched**: data type Date Only. This is used to track when we last watched something, particularly something we owned, to help us easily identify when we completely forgot about a movie we own.
- **Owned:** data type Choice, where the choice options are Blu-ray, DVD, Movies Anywhere, Google Play, Microsoft, Vudu.
- **Watchlist:** data type Choice, where the choice options are whether this movie is for just me, just my wife, together, or not on any list at the moment. This is to help prevent “Netflix cheating:” watching something independently that the other was expecting to be together.
- **Year:** data type Whole Number. This is to help distinguish when multiple movies over different years have the same title.

That’s it. It’s a pretty simple data structure. I did at times toy with more like a Streaming choice that allowed for specifying the services the movie is currently on, but cut that out and replaced it with the JustWatch link because it would be much more hassle than it’s worth to keep those up-to-date.
