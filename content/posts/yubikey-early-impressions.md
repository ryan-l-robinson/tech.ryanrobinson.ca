---
title: "Yubikey: Early Impressions"
date: 2021-10-31T19:25:55.000Z
author: Ryan Robinson
tags:
    - Security
---

I recently obtained two [Yubikey security keys](https://www.yubico.com/) to boost my personal and professional security. I picked up one Yubikey Bio and one Yubikey 5 NFC. They recommend that you always have a backup in case you lose one, and from what I had learned, I wanted the Bio in several services that would support it, but also the 5 NFC for other services and for mobile NFC authentication.

It’s now been a week, so here are some initial thoughts.

## Support

I set up on multiple services. These were the big ones to me that supported both keys:

- Microsoft (personal)
- Microsoft (Office, as long as the IT department has enabled it)
- GitHub
- 1Password
- Twitter
- WordPress

A couple others only accepted the Yubikey 5 NFC:

- Google (personal)
- GitLab

Finally, here are some of the services I tried which do have at least one form of multi-factor authentication, but did not support any kind of security key:

- LinkedIn (that one surprised me, since it is owned by Microsoft)
- PayPal
- Amazon
- Docker

The setup process in general is easier than setting up with an authenticator app or text messaging. You just need to plug it in and enter your same PIN every time, instead of going back and forth copying a numerical string from one device to another.

## Bio vs 5 NFC

The Bio, unlike the 5 NFC, has a fingerprint reader. When initially setting it up, I still had to use a PIN, but after that, I could simply tap my finger on it instead of entering the password. You still need to set a PIN on it and that will be a fall back if you can’t use the fingerprint reader, but for the most part it’s a bit easier to tap your finger.

It does not work as a fingerprint reader with Windows Hello. I thought it was supposed to, but doing some research now, nothing suggests that I can. I’ve tried it on 3 different computers (1 is work-managed) and it does not work. That’s disappointing. I thought this was a good way to get a security key and a Windows Hello device all in one.

On the other hand, there are three advantages to the Yubikey 5 NFC compared to the Bio:

1. NFC support means it will work for logging in to services on your phone, by tapping the key to the back of your phone that has NFC turned on. The Bio can’t do much for mobile logins.
2. More services support it (see above).
3. It’s cheaper.

## Conclusion

They are great. They are more secure than using an authenticator app (somebody could hypothetically hijack my authenticator app in a way they couldn’t for the key without physically taking it from me) and easier to use.

The question is whether they are great enough to justify spending on them. If you’re doing a lot of logins with apps in your work, the answer is probably yes. If you are a casual user who only has 5 or 6 services with two-factor authentication to deal with, the answer is probably no – stick with the authenticator app on your phone.

If you decide to get some, get at least two so you have a backup. Two of the Yubikey 5 NFC’s are likely all that most people need for cheaper. If you’re logging in a lot, you may want the combination like I did: a Bio for all the desktop services that support it and a 5 NFC for mobile use and the rest.
