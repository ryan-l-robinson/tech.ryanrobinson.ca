---
title: "Koa11y: Accessibility Testing Tool"
date: 2021-10-28T22:11:00.000Z
author: Ryan Robinson
description: "An overview of a desktop app for website accessibility testing."
tags:
    - Accessibility
---

[Koa11y](https://open-indy.github.io/Koa11y/) is another useful website development tool I came across recently. It is a downloadable desktop app that can be used to test out a website’s accessibility. It is not the most user-friendly app – not as intuitive as [the WAVE extension in your browser](https://wave.webaim.org/extension/), for example – but a web developer should have no problem sorting out a few extra steps. Most importantly, it did return more detailed issues with the website I first tested against compared to WAVE, which can be valuable information to fix as many accessibility issues as possible.

<iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0" loading="lazy" src="https://www.youtube.com/embed/yoJDb018Edc?feature=oembed" title="Koa11y: Find Accessibility Issues on your Site" width="500" height="320">

## Koa11y Demo

To try it out, simply download the zip file (Windows, in my case) from the link above and extract it. Launch the main executable to see a screen like this:

Enter a website that you want to run accessibility tests against. For example, I tried for ryanrobinson.technology. It will generate an HTML file saved on your computer with all the details of the report.

!["Results for the Ryan Robinson Technology site does not get a perfect score"](./ryanrobinson-results.png)

## Pa11y

It is built on top of [pa11y](https://pa11y.org/). Pa11y is a tool that iterates over a webpage and tells you all the accessibility errors found. It comes with a variety of configuration options. I first went down this route as I was investigating tools that might help me enforce more accessibility testing as part of a GitLab CI/CD pipeline. Unfortunately, it can only check websites which are publicly accessible, and our dev servers are protected and only accessible by our team. I wouldn’t have been able to integrate it into our pipeline until any changes had already gone to production, which defeated the purpose. But it did lead me to Koa11y which can still be a useful tool to have in the belt to periodically review sites which are publicly-accessible. If you’re looking for something a bit more programmatic in your workflow, though, give Pa11y a try. If Koa11y’s results are any indication, it’s a pretty powerful tool.
