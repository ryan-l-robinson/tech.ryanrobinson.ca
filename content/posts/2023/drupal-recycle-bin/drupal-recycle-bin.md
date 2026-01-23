---
title: "Drupal: Recycle Bin"
date: 2023-03-01T18:46:00-05:00
author: Ryan Robinson
description: "How to devise your own recycle bin system in Drupal"
tags:
  - Drupal
---

Drupal does not come with any kind of recycle bin / trash system out of the box, unlike the other technologies I have worked with most (WordPress, SharePoint). There were a few modules available at the time I looked into this (admittedly over a year ago now), but they were all rough, either old or barely developed. None were reliable enough to count on. In this post I’ll describe a simple halfway approach that did not require any extra code.

There is a demo of this [available on my GitHub](https://github.com/ryan-l-robinson/Drupal-recycle-bin), with a GitPod container to test it out.

## Permissions: Only Admins Can Delete

Check permissions for each content type to confirm that only admins can delete anything. Everybody else will only be able to use the flag field we're about to create to request deletion, not directly delete anything.

Go to the node permissions screen at /admin/people/permissions. Look for the permissions of each content type that are "Delete own content" or "Delete any content." For the purposes of this demo, only the admin users should have these permissions. Your permission structure may be more complicated.

!["Screenshot of the permissions screen, with all permissions given to admins and no permissions given to others. The key for this example is that only admins have the Delete permissions"](./permissions.png)

## Field: Flag for Delete

Proceed to the Manage Fields page of the first content type that you want to have this feature on. For example, if my content type is recyclable_content_type, I would go to /admin/structure/types/manage/recyclable_content_type_/fields. Create a boolean field.

!["Screenshot of the form to create a new field. A new field of type Boolean is being created with the label of Flag for Delete."](./add-field.png)

Limit the field to only allow one value at a time.

!["Screenshot of the form for setting storage on the field. This field allows only 1 value."](./field-storage.png)

The final screen for creating the field allows you to add more options that are mostly about helping your content creators understand what this is doing. I'll add some help text as well as change the On and Off labels to be more clear of what they mean.

!["Flag for Delete field settings. This includes setting the help text to explain to the users what it does, changing the On label to Delete and the Off label to Keep."](./field-help.png)

You now have the field on this content type ready to use. You may also want to go into "Manage form display" and "Manage display" to customize how this appears for your users. In my case, on the form I put it inside a Details sidebar paired with the published option. I then used a module like [condition_field](https://www.drupal.org/project/condition_field) to only show that option when the node is already marked as unpublished, so that somebody cannot accidentally mark something for deletion without unpublishing it first. Finally, on the display I hid it entirely because the public should never see that field - it is only for back-end processes.

If you have more than one content type you want to deploy this feature on, go to the next content type and add a field again. This time, select from the dropdown of existing fields instead of creating a new one. The final steps for help text, form display, and public display remain the same.

## View: Ready for Deletion

Finally, you're going to want a way that admins can review the nodes flagged for delete and go ahead with permanently deleting after the set amount of time. This is a great problem to be solved by views.

You can either create a new view or add a new display to an existing view. I'm going to start by building on top of the default admin view named "Content" since I want something that is basically the same but with extra filters. This is at /admin/structure/views/view/content. Either add a new page from the Add button, or duplicate the existing page with the Duplicate Page option in the dropdown near the right side of the screen. If there was only one display which is also the default, the result would be essentially the same.

I put it in the menu under Content of the Admin menu, and with the path /admin/content/delete.

!["First settings show the path set to /admin/content/delete. Second setting shows the Menu link as Normal and labelled Ready for Deletion."](./view-page-settings.png)

### Flag for Delete filter

Add a filter to the view, overriding this page only, on the Flag for Delete field.

!["Screenshot of the views admin to add a filter. A filter on the field Flag for Delete is being added."](./add-flag-filter.png)

!["Screenshot of the views admin, setting the value of Flag for Delete is equal to True."](./view-configure-filter.png)

### Published filter

The view I duplicated from already had a Published filter, but it was exposed to allow users to select. So I'm going to edit that one to only show unpublished content. I only want unpublished content, because if somebody has flagged for delete but it is still published, that suggests they might have made a mistake.

!["Configuration for the Published filter on the view, set as is equal to Published status of No."](./view-published-filter.png)

### Date Updated filter

Finally, in my case, I also wanted to filter on the date updated, so that only content that hasn't been updated within the past 30 days would show up. Anything less than that suggests that the content creator may be having second thoughts and isn't really ready to delete it yet. In your context, you may not want this delay at all, or you may want a much longer delay.

!["Screenshot of adding the Updated date filter to the view"](./add-updated-filter.png)

!["Screenshot of settings for the Updated filter, setting the operator to Is Less Than and the value as an offset of -30 days"](./configure-updated-filter.png)

Now admins can periodically check and bulk delete any content that is flagged for delete, unpublished, and last updated more than 30 days ago. You can also do any of the usual views changes from here as you desire, including adding views, changing text to show when there are no results, adding CSS classes, etc.
