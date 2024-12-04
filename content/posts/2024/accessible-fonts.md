---
title: "Accessible Fonts"
date: 2024-12-04T17:50:37.792Z
author: Ryan Robinson
description: "I did some research on accessibility of fonts, learning about doubles, mirrors, spacing, and what are considered some of the best."
tags:
    - CSS
    - Accessibility
---

I recently did some research on the accessibility of a variety of fonts. It's an interesting area, and it is one of those areas of accessibility where there is no one solution that is absolutely perfect for everybody. A font that is great for one particular reading disability may be a big problem for others. In other words, there are two big things to keep in mind, as always in accessibility:

1. Know your audience. If you're specifically targeting people with dyslexia, your decision will be different than for the general public.
2. Pursue an additive approach where possible. If one font can't help everyone, set a default to be something that is best for as many as possible, then have something like the [Fluid UI framework for Drupal](https://www.drupal.org/project/fluidui) which can allow users to select something else that might be better for them.

With that said, here are some of the things I learned to consider when looking at fonts:

## Testing

How did I test these? With most fonts, I could generate samples using [Google Fonts](https://fonts.google.com/) and/or [Adobe Fonts](https://fonts.adobe.com/). I typed in the scenario I was trying to test and took notes on how well they do in each of the categories below, slowly cutting down which fonts were serious contenders. As I got down to only a few options, I also tried putting them into the site I was considering fonts for so I could see it within our specific context.

## Doubles

Doubles are pairs of characters that look too similar to be able to tell about, at least without other context. In some fonts with really bad doubles, this doesn't even require any extra reading disability - it applies to all of us.

One cluster of these are the characters that are mostly straight lines in some fonts. This site won't demonstrate the problem, but in a lot of fonts, try l (lowercase letter L), the number 1, uppercase letter I, and the pipe |. There have been a handful of times in the past couple of years that I've seen so many people talking about AI, then somebody mentions a guy named Al and I am very confused because in the font of wherever I saw it, I could not tell them apart. When I did the testing, I often found that most fonts were good at distinguishing two or three of those four from each other, but not that many of them were obvious enough for all four characters.

Another very common double is the uppercase O with the number 0. In a lot of fonts, the letter is wider, but otherwise they look very similar. If it's not in the middle of a different number, you can't tell it apart. How many times you have seen an alphanumeric code like a password printed on something and not been able to tell whether it was a 0 or an O? Other than being a little skinnier for the number, the most obvious way to differentiate is to put a line through the 0, as this site's font does. Depending on the context, that might be more annoying to read than it is helpful to differentiate, but it should be something you consider.

## Mirrors

Mirrors are a little different, the pairs of letters that are distinct from each other but very similar if you flip one of them, which can be a problem for dyslexia. The two common examples are:

pq

and

db

Look at them side-by-side. Can you see what's different, maybe an extra serif on one that's not on the other to tell them apart?

## Spacing

This one isn't quite as easy to say pass or fail, but I tried to judge how readable the spacing was between letters. I found when fonts were too skinny they became hard to read, with the letters blurring together. But too far apart might also be hard to read, and also increases the pages size which results in more scrolling and that's a cognitive load in a different way. There is some sweet spot, which might be different for all of us.

## Monospace

Most fonts allow different letters to have different widths, which is another way to help them stand out from each other. There's a reason why an "em" is a unit of measurement in HTML/CSS: because the letter m is the widest letter, so it is a way of saying "I want to fit at least this many letters here."

The exception where monospace fonts - those with the same width for all letters - are often preferred is for coding. Knowing that each letter is the same width is very helpful for keeping the code blocks lined and being able to judge how many characters are there. So if you're including code snippets on your site, you might want a monospace font for those, but otherwise you probably don't.

## Conclusions

So what did I land on?

The [Braille Institute's Atkinson Hyperlegible](https://www.brailleinstitute.org/freefont/) was the best across all those tests, which makes sense given that it was very intentionally designed for accessibility. I'm using it for this site for that reason. The 0 with the strike through is a bit jarring for some, as I mentioned above, but I am starting to really like it. I've even started using it in other apps that offer it (e.g. [Pachli](https://pachli.app/download/), the Mastodon app for Android).

Tahoma scored well on most fronts, other than the 0 and O mirror. Of the major fonts by default available on most systems already, this came out as my winner. It is the most friendly to the most number of people, maybe scoring a little lowr than Atkinson Hyperlegible on some of the mirrors and doubles, but quite well on everything else, and without the line through the 0 that is a distraction to a fair number of people. In a professional context with a general audience, there's a good case to take this over Atkinson Hyperlegible.

Comic Sans, mocked by much of the Internet, is considered one of the best fonts for dyslexia, because it doesn't have mirrors. Every letter is easy to differentiate. But I think it is valid to point out that it can cause readability problems for most others, so if your site is general purpose rather than targeting the dyslexic community specifically, it might not be a great choice. There is also an Open Dyslexic that is similar, which is particularly targeted for dyslexic users.

Some others that scored generally ok: Aptos, the new Microsoft 365 default font; Arial; and Verdana (much like Tahoma but a bit wider).
