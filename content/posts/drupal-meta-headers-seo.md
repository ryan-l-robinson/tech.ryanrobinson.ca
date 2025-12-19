---
title: Drupal Meta Search Engine Headers
date: 2025-12-20T15:46:00.000Z
author: Ryan Robinson
description: "You might want to change meta tags for SEO purposes. Here's how to do that with Drupal theme preprocessors."
tags:
  - Drupal
---

## The Meta Tags

There are a few meta tags which you can put in your site to impact their search results:

- noindex: keep search engines from processing the page entirely. It won't show up in search results, at least for the major search engines that respect that tag.
- title: the title given to the search result.
- description: the description that appears under the title. This should be a maximum of 160 characters. If you don't provide one, Google/Bing/Kagi/etc will show the first 160 characters, which may or may not be what you really want to show.

## How: Theme Preprocessors

In Drupal, these meta tags can be added from the theme preprocessor, with some conditional logic to help handle variance by content type to read different fields.

### Noindex

Suppose there's one content type that I don't want to ever be indexed, because they'll only show up through views, for example, rather than going to it directly. In this example, I'll put the noindex on user pages.

```php
function my_theme_preprocess_html(array &$variables): void {
  $user = \Drupal::routeMatch()->getParameter('user');
  if ($user instanceof UserInterface) {
    $variables['page']['#attached']['html_head'][] = _my_theme_noindex();
  }
}
```

This is the helper function that it relies on, which I have separated because in my context there are multiple scenarios I want to call it.

```php
function _my_theme_noindex(): array {
  $noindex = [
    '#type' => 'html_tag',
    '#tag' => 'meta',
    '#attributes' => [
      'name' => 'robots',
      'content' => 'noindex',
    ],
  ];
  return [$noindex, 'geo'];
}
```

### Title

Only slightly more complicated: suppose there's a node content type for events where I want to add "Event" to the start of the title to help make it clear that this is a time-limited event.

```php
function my_theme_preprocess_html(array &$variables): void {
  $node = \Drupal::routeMatch()->getParameter('node');
  if ($node instanceof NodeInterface) {
    $type = $node->getType();
    switch ($type) {
      case 'event':
        $variables['head_title']['title'] = "Event: " . $variables['head_title']['title'];
        break;

      default:
        break;
    }
  }
}
```

### Description

Let's stick with that event scenario and get a little more complicated again. For an event, I want to make sure the summary shown to social media includes the date of the event, as well as some text from a summary field.

```php
function my_theme_preprocess_html(array &$variables): void {
  $node = \Drupal::routeMatch()->getParameter('node');
  if ($node instanceof NodeInterface) {
    $type = $node->getType();
    switch ($type) {
      case 'event':
        if ($node->hasField('field_start_date') && isset($node->get('field_start_date')?->value)) {
          $description .= "Event time: ";
          $start_date_value = $node->get('field_start_date')->value;
          if (is_string($start_date_value)) {
            $start_date_time = new DateTime($start_date_value, new \DateTimeZone('UTC'));
            $start_date_time->setTimezone(new \DateTimeZone(date_default_timezone_get()));
            $start_date_timestamp = intval($start_date_time->format('U'));

            if ($node->hasField('field_end_date') && isset($node->get('field_end_date')?->value)) {
              // If start and end have same date, format cleanly.
              $start_date_only = \Drupal::service('date.formatter')->format($start_date_timestamp, 'medium');

              $end_date_value = $node->get('field_end_date')->value;
              if (is_string($end_date_value)) {
                $end_date_time = new DateTime($end_date_value, new \DateTimeZone('UTC'));
                $end_date_time->setTimezone(new \DateTimeZone(date_default_timezone_get()));
                $end_date_timestamp = intval($end_date_time->format('U'));
                $end_date_only = \Drupal::service('date.formatter')->format($end_date_timestamp, 'medium');

                $description .= ($start_date_only == $end_date_only) ? Drupal::service('date.formatter')->format($start_date_timestamp, 'long') . " to " . \Drupal::service('date.formatter')->format($end_date_timestamp, 'custom', 'g:i a') : Drupal::service('date.formatter')->format($start_date_timestamp, 'long') . " to " . \Drupal::service('date.formatter')->format($end_date_timestamp, 'long');
              }
            }
            else {
              $description .= \Drupal::service('date.formatter')->format($start_date_timestamp, 'long');
            }
          }
        }
        $description .= ". ";
        break;

      default:
        break;

      // All types have the same summary field.
      if ($node->hasField('field_page_summary') && !empty($node->get('field_page_summary')->value)) {
        $description .= $node->get('field_page_summary')->value;
      }
      if (!empty($description)) {
        $variables['page']['#attached']['html_head'][] = _my_theme_description($description);
      }
    }
  }
}
```

That again relies on a helper function that cleans up any special characters and trims it to 160 characters with some ellipses afterward. Here's that function, which I used to sanitize the final result of multiple content types.

```php
/**
 * Helper function to generate description tag.
 *
 * @param string $description
 *   String for the description to put into the metatag.
 *
 * @return array
 *   Meta description tag to be added as a header.
 */
function _my_theme_description(string $description): array {
  $description = html_entity_decode(strip_tags($description));
  if (strlen($description) > 160) {
    // Cut at new line, maximum 155 characters.
    $length = strpos(wordwrap($description, 155), "\n");
    if (is_int($length)) {
      $description = substr($description, 0, $length) . "…";
    }
  }
  $description = trim($description);
  $meta = [
    '#type' => 'html_tag',
    '#tag' => 'meta',
    '#attributes' => [
      'name' => 'description',
      'content' => $description,
    ],
  ];
  return [$meta, 'geo'];
}
```
