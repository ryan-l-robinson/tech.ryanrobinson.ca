---
title: "WordPress: Accessibility Basics"
date: "2021-04-16T08:59:00-04:00"
author: "Ryan Robinson"
tags:
  - WordPress
  - Accessibility
---

Accessibility is an important part of designing a website. You want your site to be usable to as many people as possible, right? Here are some things to consider as you develop a WordPress site to make it as accessible as you can.

## Testing with WAVE

The best tool for testing out a website is the WAVE evaluation tool, which is available as an extension for your browser. It will scan the code of the page and tell you any accessibility problems it finds, flagging them with different levels of seriousness. It also has a section for contrast issues when the foreground colour of text is too close to the background colour.

Note that with WordPress WAVE will always pick up a few issues from the admin toolbar if you are logged in when you run it. That’s not great, but it’s out of the control of the site developer and it doesn’t impact regular visitors.

## Pick a Theme

The theme decision will determine a lot of your site’s accessibility. Some themes have a lot of accessibility problems built right into them and it will be hard for you to change the code yourself to fix it. Take your time shopping for themes. Activate a theme and use the WAVE accessibility tool to see how well it scores. If there are little things like missing ALT text, you might be able to work around that without changing the theme, but be cautious if it has a lot of issues like inconsistent headers or a search bar without a label.

You may not find a theme that’s perfect, but if you put in the time, you’ll at least get pretty close.

## Contrasting Colours

Picking a colour scheme of a site goes beyond simply looking good. There needs to be strong enough contrast between the background colour and the foreground text colour to be sure that people with low vision can differentiate.

There are levels to this. Hitting the most strict AAA sometimes can be hard to do in light of other factors like brand consistency, but you want to make sure you at least hit the AA standard. The WAVE tool can help measure your contrast and even help you pick colours for stronger contrast.

## The WP-Accessibility Plugin

Sometimes if your theme doesn’t do a great job hitting all the accessibility requirements, the WP-Accessibility plugin can help. It includes a few helpful tools like:

- Adds the title of the blog post to the “read more” text. This helps for people with screenreaders. When all the links say “read more,” a screenreader jumping through links just says “read more” over and over again, which isn’t helpful. It’s a lot more helpful when it says “read more Accessibility in WordPress.”
- Adds an accessibility toolbar on the side of your site for adjusting font size and contrast.
- Attempts to add a label to the search bar, although I’ve had mixed success with that option.

## Use the Right Headers

That covers the general site setup. Now we’re into features that anybody should consider when writing content like this blog post.

Many people misuse header tags. They are not solely for formatting. They also help identify how content is related to each other. Your site should have an h1 tag, either for the site title or page title. Then everything below that should be an h2. Content that is a sub-category of an h2 should be identified with an h3. And so on. You shouldn’t skip tags from an h2 down to an h5, for example, or always use h2 no matter what without considering what should maybe be lower.

## (Almost) Always Add ALT Text

This is the accessibility issue that most often gets discussed, but it’s still very easy to miss in WordPress. ALT text is a technique to describe what a screenreader says to a user that can’t see the picture. You should always set ALT text unless the photo is purely decorative like an icon.

WordPress offers the option to set ALT text, but it doesn’t require it. It’s also not quite as in-your-face in the new Gutenberg block editor as it used to be in the classic WordPress editor, so it’s easy to add an image and forget to dig into the settings for the ALT text option.

Update: in 2023 I wrote [another post discussing how descriptive ALT text should really be](/posts/2023/descriptive-alt/), and in some cases you may not want ALT text at all.