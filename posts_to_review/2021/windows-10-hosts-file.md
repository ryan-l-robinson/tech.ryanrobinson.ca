---
title: "Windows 10: Hosts File"
date: 2021-11-29T13:55:00.000Z
author: Ryan Robinson
tags:
    - Visual Studio Code
---

Configuring a hosts file on your computer allows your browsing traffic to go to a different server than is listed by public DNS. This can be essential for a few scenarios, such as:

1. migrating a site to a new server and needing to test it before changing the public DNS
2. development on dev/staging servers which do not have public DNS listings

## Open the File

The hosts file in Windows is located at C:\\Windows\\System32\\drivers\\etc\\hosts. I find I routinely forget that, so I added a shortcut to my primary document folder so I can get back to it faster. To do that in Windows, browse to the folder where you want the shortcut. Right-click and select New Shortcut. In the prompt that opens, enter C:\\Windows\\System32\\drivers\\etc\\hosts for the location of the item. Click next, then provide it a shortcut name – that could stay simple as “hosts” or something else. Save it, and now you’re good to go.

Side note: Linux has a similar system for overriding public DNS. The hosts file there is typically located at /etc/hosts and is structured the same way.

You will need to edit this file as an administrator in order to make changes. You can approach this either way:

1. Open your text editor as an administrator initially by right-clicking on the icon to launch it and select “Run as administrator.”
2. Some editors such as Visual Studio Code will allow you to open the file first as a non-administrator, then when you try to save it will fail and prompt you if you want to save it as an administrator instead.

Since I use VS Code for my main editor anyway, I tend to use option 2 as a bit faster most of the time.

## File Contents

The contents of the file are straightforward. Each line has an IP address and at least one domain. When you view that domain in your browser, it will go to that IP address instead of checking public DNS. For example, if you want to browse your local VM at <https://domain1.com>, add this line:

```text
127.0.0.1 domain1.com
```

If you want to have multiple domains on the same IP address, like a dev server that has a couple different test sites on it, you can structure it in one of two ways. The shorter version has one line per IP address, followed by all the domains:

```text
127.0.0.1 domain1.com domain2.com domain3.com
```

You can also structure it with a new line for each domain, even if that means repeating the IP:

```text
127.0.0.1 domain1.com
127.0.0.1 domain2.com
127.0.0.1 domain3.com
```

If you’re manually editing the file, you’re probably going to do the first one, since it is shorter. But the latter can be useful if you’re automating scripts that would benefit from adding a single domain as needed.

That’s it! It’s straightforward once you know how to do it, but it’s one of those things that is (understandably) hidden from the average user so you likely never hear about it without looking for it.
