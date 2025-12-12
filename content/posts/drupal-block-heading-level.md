---
title: Drupal Block Heading Level
date: 2025-11-26T16:19:16.700Z
author: Ryan Robinson
description: I made a module to make it easier to change the heading level on the label for blocks.
tags:
  - Drupal
  - PHP
---

## The Problem

Heading levels are important, for accessibility as well for site style consistency. However, it's not always easy to select the correct heading level in all aspects of Drupal. Specifically, it's not the easiest to change the heading level on blocks, which could vary depending on where in the site theme the block is going to be placed.

There is the twig template system and the heading level is usually hardcoded in there. If you only have one block template file and you want the heading level to always be the same, that's pretty easy to set it in one template file. Once you lose those two assumptions, though, that gets more tricky. It's still possible, but if you're changing a lot of blocks, you could end up with a mess of files that are harder to keep track of and to implement any future changes to blocks because some of the changes need to go to all the template files, some of them to one template file, some of them to some other subset in between.

## The Solution

Instead of having that mess of template files, I decided to make a module that allows changing the heading level in configuration. [The whole module is available in my GitHub](https://github.com/ryan-l-robinson/drupal-block-label-heading).

### Site-Wide Default

There's a setting for a site-wide default. It's probably true that most blocks on the site are going to need to be an h2 or an h3, and this allows setting that default to make the rest easier from there.

Here's the config schema for that option:

```yml
block_label_heading.settings:
  type: config_object
  label: "Block Label Headings settings"
  mapping:
    heading_level:
      type: label
      label: "Site's default heading level"
```

The form to change that is in src/Form/BlockLabelHeadingSettingsForm.php, which includes these functions to build and submit the form:

```php
  /**
   * {@inheritdoc}
   */
  public function buildForm(array $form, FormStateInterface $form_state): array {
    $config = $this->config('block_label_heading.settings');
    $form['heading_level'] = [
      '#type' => 'select',
      '#title' => $this->t('Default Heading Level'),
      '#default_value' => $config->get('heading_level') ?? 'h2',
      '#description' => $this->t("This can be overridden per block."),
      '#options' => [
        'h2' => 'H2',
        'h3' => 'H3',
        'h4' => 'H4',
        'h5' => 'H5',
        'h6' => 'H6',
      ],
    ];

    return parent::buildForm($form, $form_state);
  }

  /**
   * {@inheritdoc}
   */
  public function submitForm(array &$form, FormStateInterface $form_state): void {
    parent::submitForm($form, $form_state);

    $this->config('block_label_heading.settings')
      ->set('heading_level', $form_state->getValue('heading_level'))
      ->save();

    // Clear all blocks to reflect any change.
    Cache::invalidateTags(['block']);
  }
```

### Per-Block Setting

Then there is an option per block to override that default. The schema for that looks like this:

```yml
block.block.*.third_party.block_label_heading:
  type: mapping
  label: "Block label heading settings"
  mapping:
    heading_level:
      type: string
      constraints:
        Choice:
          choices: ["h2", "h3", "h4", "h5", "h6"]
```

This adds the option in the third_party section for any block.

The form is updated with a hook in the .module file:

```php
/**
 * Implements hook_form_block_form_alter().
 *
 * Adds the option to the block form.
 */
function block_label_heading_form_block_form_alter(array &$form, FormStateInterface $form_state, string $form_id): void {

  $form_object = $form_state->getFormObject();
  if ($form_object instanceof BlockForm) {
    /** @var \Drupal\block\Entity\Block $block */
    $block = $form_object->getEntity();

    // Pin the Title fields at the very top with explicit weights,
    // so the new option will be grouped with it.
    if (isset($form['settings']['label'])) {
      $form['settings']['label']['#weight'] = -100;
    }
    if (isset($form['settings']['label_display'])) {
      $form['settings']['label_display']['#weight'] = -99;
    }

    $options = [
      'h2' => 'H2',
      'h3' => 'H3',
      'h4' => 'H4',
      'h5' => 'H5',
      'h6' => 'H6',
    ];

    $form['settings']['block_label_heading_level'] = [
      '#type' => 'select',
      '#title' => t('Heading level for block title'),
      '#options' => $options,
      '#default_value' => $block->getThirdPartySetting('block_label_heading', 'heading_level', 'h2'),
      '#description' => t("Choose the semantic heading level used to render this block's label, as is most appropriate for where it will be placed in the site."),
      '#weight' => -98,
    ];

    $form['#entity_builders'][] = 'block_label_heading_block_entity_builder';
  }

}
```

### The Block Template

The last step is to get those configured headings into the displayed template, without needing to hardcode a separate template for every block.

The module will pass a variable through to twig using this hook implementation:

```php
/**
 * Implements hook_preprocess_block().
 *
 * Adds variable for templates to customize block heading level.
 */
function block_label_heading_preprocess_block(array &$variables): void {
  $block_id = $variables['elements']['#id'] ?? NULL;
  $variables['heading_level'] = \Drupal::service('block_label_heading.utils')->getHeadingLevel($block_id);
```

The last step cannot be done by the module, as it is to change the theme implementation.

Now that you have that heading_level variable that you can use in a twig template, the label portion of your block template file may look something like this:

{% raw %}

```twig
{% if label %}
  {% set tag = heading_level|default('h2')|lower %}
  {# Avoid aria-level conflicts injected elsewhere. #}
  {% set title_attributes = title_attributes.removeAttribute('aria-level') %}
  <{{ tag }}{{ title_attributes.setAttribute('id', heading_id) }}>{{ label }}</{{ tag }}>
{% endif %}
```

{% endraw %}
