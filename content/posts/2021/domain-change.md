---
title: 'Domain Change'
date: '2021-09-19T15:46:56-04:00'
author: 'Ryan Robinson'
description: "I've changed the name for this site as I move away from Alliteration Applications branding."
---

This site is now ryanrobinson.technology instead of alliterationapplications.com. I felt that it was more representative of what this site has evolved to become. It is a repository of my technology knowledge and experience. It isn’t just about the rare freelance work that I do under the Alliteration Applications banner. Many of the blog posts came from personal study, personal projects, and (generalized versions of) work done within a full-time job – not from Alliteration Applications at all.

With that rationale out of the way, I thought I’d break down how I changed the URL. I believe I could have done this a bit simpler by changing the URL on the same site, rather than making a copy first, but I decided to err on the side of caution where I would always have the old version if anything went wrong.

## Rearrange Content

I won’t get into all the details, but I had to rearrange a bunch of the content to make sense with the new broader goal for the site: the title of the site, the content of the homepage, and so on.

## Change Domain A Record

To tell the wider Internet what to do when they try to visit ryanrobinson.technology, I had to go into my DNS records for the domain. It was previously using a simple FWD record to redirect any traffic to it to alliterationapplications.com, but now I want essentially the opposite. ryanrobinson.technology needs to be hosted and alliterationapplications.com needs to redirect traffic away instead. So, I removed the FWD record and added an A record to my IP address instead.

## Add Domain to Server Account

I’m on a shared hosting service with a cPanel. Step one was to add the extra domain ryanrobinson.technology to the same account as alliterationapplications.com. I specified that I wanted it to be a different document root than the existing site.

I also took advantage of the AutoSSL feature in cPanel to generate an SSL certificate for the new domain.

## New Database

I created a new database and new database user for the new site installation.

## Duplicator

I used [Duplicator](https://wordpress.org/plugins/duplicator/) to make a copy of the site and install it in a new folder.

At this point I had a functioning site at both domains.

## .htaccess on Old Domain

I then wanted to redirect traffic from alliterationapplications.com to ryanrobinson.technology, and I didn’t want to simply redirect everything to the homepage. I wanted specific URLs to go to the matching URL.

To achieve that, I added this to the .htaccess of alliterationapplications.com

```apache
<pre class="wp-block-code">```
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteRule ^(.*)$ https://ryanrobinson.technology/$1 [R=301,L]
</IfModule>
```

## Cleanup

Finally, I cleaned up everything from alliterationapplications.com that I no longer needed: the database and all the files in the alliterationapplications.com document root other than the .htaccess file. That’s not truly necessary, but does keep my storage usage down.
