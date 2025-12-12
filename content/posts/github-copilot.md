---
title: GitHub Copilot
date: 2021-10-23T01:37:15.000Z
author: Ryan Robinson
description: "Some thoughts about GitHub Copilot."
tags:
    - Git, GitLab, and GitHub
    - PHP
    - Visual Studio Code
---

A few months ago it was big news in the programming world when [GitHub Copilot](https://copilot.github.com/) was announced. This week, I was accepted into the preview, so now I have a chance to try it out and share my thoughts.

## Concerns

Before I do that, I’ll make a few caveats. When it was announced, the reaction was mostly positive. It is an incredible service that can really help speed up development. But there were a couple concerns I saw being raised, which I will address quickly.

One concern was that developers would allow Copilot to write large chunks of their code and not actually understand it. I’m sure this will happen and it is certainly not ideal. It’s also not that different than copying and pasting from StackOverflow without understanding. That’s why ultimately this part doesn’t concern me much.

The other concern I think is a bit more serious: licensing. Copilot combines code from publicly-available code repositories. Those repositories may have a wide variety of licenses on them. Some are completely fine with you using them in a commercial product. Some do not want you to do that at all. Some are ok with it as long as you provide credit. The problem with Copilot grabbing the code for you is that it does not filter by license and it doesn’t even point you to the original repositories that helped inspire the code. The counterpoint is that Copilot is not strictly copying code from the repositories – it is writing new code with inspiration from many repositories – but it still feels a bit closer to the ethical line than many are comfortable with.

## My First Test

With that out of the way, I’ve now had a chance to try it out.

The first thing I tried was in a WordPress plugin that looks for Bible references and wraps them in links. I’ll have a post about this when it’s done, but I did this years ago in a previous job and decided to try to recreate it as a portfolio project.

I had two major problems left when I got access to Copilot. The first was easy but would take some time: I needed to list all the books of the Bible and all the common abbreviations for them. I did have a shell of a function already, but only had done anything for Genesis and Exodus. So, I wrote a comment saying this is exactly what I needed to accomplish.

```php
/**
 * Define abbreviations for all books of the Bible
 */
```

Copilot kicked in. Sort of. It gave me what looked like it was exactly a slightly-older version of my code right above it, only giving me Genesis and Exodus. I can say it was kind of cool to watch the suggestion pop up, even if that excitement faded quickly once I realized it wasn’t giving me anything helpful.

The second was regex. I needed it to recognize a few different potential patterns. I also needed it to ignore if the link was in a header or another link. Similar to the first problem, I did already a shell of an idea here, but I tried to get it to help me. This time it gave me nothing useful at all – it didn’t even try to write regex.

Maybe this use case was too niche, although I imagine there are enough other uses on GitHub for a list of Bible abbreviations. Maybe it was struggling because I had already started something previously and so it stuck to that instead of starting fresh. I’m certainly not giving up on the idea. I’ll leave it enabled as I do more work. Hopefully it will offer something a bit more valuable as time goes on, and if it does, I’ll try to blog about that. For now, my grade of Copilot has to be an “incomplete.”

## Update

Since this initial test, I have kept Copilot turned on for more normal operations. It has helped a little with Drupal PHP work, but not much. It has helped more with CSS: sometimes it will generate something for me and I’ll try it out and adjust from there, rather than writing my own from scratch. It has prompted a good idea or two for me that way. The most I’ve found I’ve accepted the suggestions is actually not in the code itself but in writing comments; it is quite good at finishing my sentences. So, it continues to not be as dramatic for me as I’ve seen others demo, but it is certainly worth turning on.
