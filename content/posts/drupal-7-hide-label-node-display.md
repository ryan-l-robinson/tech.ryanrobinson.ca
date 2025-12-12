---
title: "Drupal 7: Hide Label on Node Display"
date: 2022-01-05T00:02:00.000Z
author: Ryan Robinson
description: "How to hide an empty field's label in a Drupal 7 page template."
tags:
  - Drupal
  - PHP
---

I ran into a problem on an old Drupal 7 site where labels for a custom field were displaying, even when there was no content associated with that field, creating a page that was simply a series of headers in a row. That’s not very user-friendly.

If I wanted to always hide the label for a field, that’s easy enough in the default Drupal display settings for the content type. But what about this scenario where we want to show the label when there is content and hide the label when there isn’t content?

In theory, the logic for this is simple. In the node template for that content type – in the node–content-type.tpl.php in the templates folder of your theme – we want to add some simple logic that says to not show the field at all if the value of the field is empty.

If you don’t already have a node–content-type.tpl.php, copy your default node.tpl.php file, rename it to node–content-type.tpl.php, and then the changes you’ll want to add are before the main content variable is rendered for the page.

A couple wrinkles did make it a bit more complicated, though.

First, you cannot check if the value of the field is set. It is always set. That’s entirely reasonable, but not obvious, so I still mention it.

Second and a lot less reasonable, I couldn’t even use a test for if the field is empty. This is where I spent the bulk of my time being confused. I would write the value of the field to a log or message to test and it should show nothing. I could inspect the code and there was nothing – not even a space or a hidden HTML tag. But if I tried to test for empty() or the string to equal ”, it still said no, it’s not empty. Eventually I checked the length of that string and discovered it was 2, not 0.

I do not know if that is universal in Drupal 7 or if there was something weird with this one site. It’s possible you’ll be able to get away with a simple empty() test instead of the strlen() test.

So this was my final code addition to the node template file:

```php
//hide some labels if the corresponding value is empty

$field_value = $content['field_field_name']['#object']->field_field_name['und'][0]['value'];

if ((strlen($field_value) <= 2)) {
  unset($content['field_field_name']);
}
```

You could also do this in one line instead, depending on where you fall in the debate between easy readability vs minimal code. I left it as two different lines to be easier for somebody looking at it to understand what was happening.

```php
//hide field label if the corresponding value is empty

if ((strlen($content['field_field_name']['#object']->field_field_name['und'][0]['value']) <= 2)) {
  unset($content['field_field_name']);
}
```
