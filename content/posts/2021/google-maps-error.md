---
title: "This Page Can't Load Google Maps Correctly"
date: "2021-05-17T15:54:00-04:00"
author: "Ryan Robinson"
description: "Dealing with an error about loading Google Maps embedded in a site."
---

You may be browsing a website – your own or somebody else’s – and see this error:

> “This page can’t load Google Maps correctly. Do you own this website?”

It also has “For development purposes only” as a watermark repeating over the whole map.

This error has become pretty common in the past few years. It comes out of a change in Google policies. Previously, you could use the Google Maps API to put a map on your website with no extra charge. You did not need to have a credit card on the Google account. That has changed. There’s no notification, so you won’t notice this unless you happen to visit the map on your website again, which most people rarely do after it’s been designed.

You now need to meet two requirements:

1. An API key on the site connecting to a project associated with your Google account\*. If you’re building a Google Map into a CMS website (Drupal, WordPress, etc.) then it probably includes some instructions on how to do this connection.
2. A valid payment method on the Google account. You can do this from pay.google.com. The billing account of the Map API project has to be connected to that payment method.

As with most other scenarios like this, you should have a generic Google account that can be shared by at least a couple of staff members. Do not use an individual user’s Google account – personal or business – as that will cause complications if you ever need to make changes and that person isn’t available.

Importantly, you get a certain amount of API credit for free before you have to start paying. Most small to medium websites will likely end up not paying any extra fee. That’s at least true of Google’s rules today; they could easily change someday. You do need to be aware of that slight possibility.
