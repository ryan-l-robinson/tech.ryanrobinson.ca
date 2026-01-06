---
title: H5P Views Drupal Module
date: 2026-01-06T14:54:16.700Z
author: Ryan Robinson
description: "This Drupal module extends H5P to expose more data to views."
tags:
  - Drupal
  - PHP
---
## The Problem

H5P is a tool that is used to help build interactive content in education settings. It has [a Drupal module](https://www.drupal.org/project/h5p) so that you can host your own interactive objects on a Drupal site. Those interactive objects can be of [a variety of H5P library types](https://h5p.org/content-types-and-applications) and then can be embedded elsewhere like on other websites or inside a Learning Management System implementation.

It is not always the best maintained module, though, and doesn't have some things that I wanted. One of those missing features is exposing the H5P library type within views, to display as well as to filter or sort by them.

## The Solution

To help meet that need, I developed a custom module. [It is available on my GitHub](https://github.com/ryan-l-robinson/drupal-module-h5p-views).

There are two main components for this to work:

### The Hooks to Expose the Data

Since I am now working primarily with Drupal 11.3, I will use [the new OOP hook implementation system](https://attia-it.com/blog/hooks-implementation-drupal-11-new-oop-approach), putting a file under src/Hook/H5pViewsHooks.php:

```php
<?php

namespace Drupal\h5p_views\Hook;

use Drupal\Core\Hook\Attribute\Hook;
use Drupal\Core\StringTranslation\StringTranslationTrait;

/**
 * Implements hooks for H5P Views module.
 */
class H5pViewsHooks {

  use StringTranslationTrait;

  /**
   * Adds fields for views.
   */
  #[Hook('views_data')]
  public function addH5pViewData(): array {
    // Finding the data through a join to another table.
    $data['h5p_libraries']['table']['join'] = [
      'h5p_content' => [
        'left_field' => 'library_id',
        'field' => 'library_id',
      ],
    ];

    // Definition of the type of data.
    $data['h5p_libraries']['title'] = [
      'group' => $this->t('H5P Content'),
      'title' => $this->t('H5P Library Type'),
      'help' => $this->t('The H5P library of this content'),
      'field' => [
        'id' => 'standard',
      ],
      'filter' => [
        'id' => 'h5p_library_title',
      ],
      'argument' => [
        'id' => 'standard',
      ],
      'sort' => [
        'id' => 'standard',
      ],
    ];

    return $data;
  }

}
```

This tells Drupal when it is looking for possible data to include in views that the H5P library type should be one of the pieces of data available, including first establishing the relationship between the table of H5P content with the table of H5P libraries.

Most of that is straightforward if you've exposed data to views before, but the one unique line is the filter ID being set to h5p_library_title. That is referencing a custom filter that is going to be defined next.

### The H5P Titles Filter

The filter plugin is defined in src/Plugin/views/filter/H5pLibraryTitleFilter.php:

```php
<?php

namespace Drupal\h5p_views\Plugin\views\filter;

use Drupal\Core\Database\Connection;
use Drupal\Core\Database\StatementInterface;
use Drupal\Core\Plugin\ContainerFactoryPluginInterface;
use Drupal\Core\StringTranslation\StringTranslationTrait;
use Drupal\views\Attribute\ViewsFilter;
use Drupal\views\Plugin\views\display\DisplayPluginBase;
use Drupal\views\Plugin\views\filter\InOperator;
use Drupal\views\ViewExecutable;
use Symfony\Component\DependencyInjection\ContainerInterface;

/**
 * Exposed filter that lists only H5P library titles which have content.
 *
 * @ingroup views_filter_handlers
 */
#[ViewsFilter('h5p_library_title')]
class H5pLibraryTitleFilter extends InOperator implements ContainerFactoryPluginInterface {
  use StringTranslationTrait;

  /**
   * Database connection.
   */
  protected Connection $connection;

  /**
   * {@inheritDoc}
   */
  public static function create(ContainerInterface $container, array $configuration, $plugin_id, $plugin_definition): H5pLibraryTitleFilter {
    /** @var static $instance */
    $instance = parent::create($container, $configuration, $plugin_id, $plugin_definition);
    $instance->connection = $container->get('database');
    return $instance;
  }

  /**
   * Build the list of selectable options.
   *
   * Keys are the actual filter values, labels are shown in the dropdown.
   */
  public function getValueOptions(): array|null {
    $options = [];

    $connection = $this->connection;
    if ($connection instanceof Connection) {
      $query = $this->connection->select('h5p_libraries', 'l');

      // Join must be on its own line (it returns the alias).
      $query->innerJoin('h5p_content', 'c', 'c.library_id = l.library_id');

      // Select fields and other clauses on the query object.
      $query
        ->fields('l', ['title', 'library_id'])
        ->distinct()
        ->orderBy('l.title', 'ASC');

      $statement = $query->execute();
      if ($statement instanceof StatementInterface) {
        foreach ($statement->fetchAllAssoc('library_id') as $row) {
          $options[$row->title] = $row->title;
        }
      }

    }

    $this->valueOptions = $options;
    return $this->valueOptions;
  }

  /**
   * Force a dropdown instead of checkboxes as a sensible default.
   *
   * Editors can still change widget type in the Views UI.
   */
  public function init(ViewExecutable $view, DisplayPluginBase $display, ?array &$options = NULL): void {
    parent::init($view, $display, $options);
    $this->valueFormType = 'select';
  }

}
```

This accomplishes two things.

The big one is the getValueOptions function. This function determines which options are available in the dropdown. It will query content from the database, get associated H5P library types, then join that to the table with all the details about the libraries so that we can get to the friendly display name. It will treat multiple libraries as the same if they have the same title, which we want because with H5P libraries you can keep older versions when the new ones are installed and there might even be content still using the old library version. For the purposes of a filter we want those to be treated as if they are the same.

The second is much simpler: the InOperator class that this is extending from will default to showing those options as checkboxes. I want it to default to a dropdown ("select") instead.

### Conclusion

With those two core pieces, plus a schema file and the obligatory info.yml file, are all that you need in terms of code. Now when editing a view of H5P content, you can show the library type, including with a friendly dropdown of the types in an exposed filter.
