---
title: Tel Links
date: 2022-05-04T01:46:00.000Z
author: Ryan Robinson
tags:
  - Accessibility
---

For many years there has been a standard for making links that open phone apps to dial a phone number. This is really great. Imagine you're browsing a website for a pizza place and you want to start a phone call to place the order. Without a tel link, especially on mobile, you're stuck memorizing the number and copying it over to your phone app. With a tel link, you quickly click on the link and your phone app is opened with the number in the dialpad.

The problem comes with extensions. How do you tell the phone app to dial an extension after the base number?

Some quick searching of the Internet seems to point toward the use of w (for wait) or p (for pause) as the marker between the number and the extension, e.g. 555-555-5555w555 or 555-555-5555p555. I tested these options on my Android phone and on Skype on a Windows 10 machine (the version from the Store). They did not work.

Some more research found that another option was a comma (,). This one does work on both iOS and Android. It does not work with desktop Skype. 

It also might not cover absolutely everything that has to be pressed. For example, in the system I tested in, it required pressing # after the extension. It did not work to add # to the end of the link, e.g. tel:+1555-555-5555,555#

## Now What?

So, what should you do? It's not very consistent, unfortunately. If you know your audience is solely or mostly iOS and Android, you can proceed with the comma approach. But that will fail completely - not gracefully - in other uncommon circumstances like Skype. That might be an acceptable trade most of the time, because how many people are really clicking on a phone link on something other than iOS or Android at this point?

A middle ground approach is to link the base phone number without the extension. This will work on any phone app that supports the tel protocol. This leaves some minor room for confusion, though: somebody clicks the link, it launches the phone app, then you hear it asking for an extension, so you have to switch back to the browser to read the extension, and switch back to the app to enter it. That's a bit of an annoying surprise, but I'm in the camp that it is still less annoying than having no link at all.

And of course, that's the third option: don't link anything. This would be the philosophy that it's better to do nothing if you can't do it perfect.

In my context, we went with the comma. We know the audience is going to be almost if not entirely iOS and Android wanting that functionality. But which one you go with may depend on your context, because unfortunately it is not a completely universal standard.
