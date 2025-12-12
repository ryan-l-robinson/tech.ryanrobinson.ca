---
title: "SMTP Through a Google Account"
date: "2021-04-28T15:55:00-04:00"
author: "Ryan Robinson"
tags:
  - CRM
---

Many applications like business scanners or hosted CRM systems come with features that send email. To do so, it has to be able to connect to an email account that it sends on behalf of. The main challenge is that most good modern email services are strict about allowing emails to be sent on their behalf. Microsoft 365 is particularly strict. Google is a bit easier, but does require an extra step which is not obvious.

First, decide which Google account you’ll want to do this. I recommend that it is an account solely for this purpose. **It definitely should not be a regular user account that might contain sensitive data**. The reason is what will come up below: by nature of this functionality you will need to disable some of the advanced Google security so that [it is protected only by a password](/posts/2021/security-essentials-multi-factor-authentication/) (generally a bad idea). Create a specific Gmail account for this purpose, like [company name].crm@gmail.com.

## Configure Settings

Next, you need to find the options in the application trying to send the email. Look for something with “SMTP” in it. It could also be something more generic like “outbound email.”

Once you find those settings options in the system that you want to be able to send from as your Gmail, fill in the valued needed for Gmail. [The values are mostly easy to find on the Internet](https://support.google.com/mail/answer/7126229?hl=en), but I’ll copy them here as well:

- SMTP Server: smtp.gmail.com
- Requires SSL: yes
- Requires TLS: yes (if available)
- Requires authentication: yes
- Port for SSL: 465
- Port for TLS (if available): 587

## Less Secure Access

There’s one more important step that is not obvious. By default, Google accounts do not allow you to access by SMTP over anything it deems a “less secure app.” Your CRM or scanner connecting over SMTP will probably be considered a less secure app.

So, you’ll also need to disable that option. You can do that here when logged in: [Less Secure Apps option](https://myaccount.google.com/lesssecureapps). Note that you’ll also have to turn off two-factor authentication to be allowed to do this.

Once that’s done, go ahead and test to confirm it worked.
