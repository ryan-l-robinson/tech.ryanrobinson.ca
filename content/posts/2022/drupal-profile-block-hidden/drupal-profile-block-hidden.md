---
title: "Drupal: Hide Block on Own Profile"
date: 2022-07-12T20:46:00.000Z
author: Ryan Robinson
tags:
  - Drupal
---

Recently I was building a Drupal 9 site that included staff profiles as well as contact forms for those staff members. I wanted to add a block on the profile pages that included a button to link to the contact form for this contact.

Here's the wrinkle in the default behaviour: it won't give you a contact link when you're viewing your own content, but it also isn't returning as nothing at all, so the title for the block appeared with nothing beneath it.

![Screenshot of empty box only saying the title "Email"](./empty-email-sidebar.png)

The key to solve this is the "exclude" option on contextual filters. There's one contextual filter for the user ID associated to the profile that you're viewing.

![Screenshot of the configuration page. Details below](./only-matching-user.png)

To add this:

- Add a contextual filter searching for the User ID
- Set the default value to "User ID from profile route match"

I needed another contextual filter for the user ID of the currently logged in user, and mark that one as excluded.

![Screenshot of the configuraton page. Details below.](./exclude-current-user.png)

To add this:

- Add a contextual filter searching for the User ID
- Set the default value to "User ID from logged in user"
- Under the "more" section, check the "exclude" option

Finally, change the option on the view in the Advanced -> Other section for "Hide block if the view output is empty" to Yes. This will turn off the block entirely when there is no results, such as if you're viewing your own profile when that contextual filter is in place.

That did the trick! The block will now appear on any profile where the associated contact has a personal contact form, unless you're viewing your own profile.
