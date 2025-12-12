---
title: "Drupal: Assign Permissions Based on Username File"
date: 2022-01-25T01:20:00.000Z
author: Ryan Robinson
description: "How to assign permissions based on whether a username is in a file."
tags:
  - Drupal
  - PHP
---

Here’s a recent scenario I encountered: a Drupal role needs to be assigned to certain users. The site is using a single sign on (SSO) system with a lot of users who could log in. But only some of those should be granted a certain permission. The list of those who can access the special permission role is automatically generated and put in place on the server on a daily basis, in a simple format with one line for each account name.

I have pulled out a generic version of this code and [made it available on my GitHub](https://github.com/ryan-l-robinson/drupal-permissions-from-file).

## Hook on Login

First, we need to hook on login. When a user signs on, we want to find out if they should have the extra permission. This can be done with [the hook\_user\_login hook](https://api.drupal.org/api/drupal/modules%21user%21user.api.php/function/hook_user_login/7.x).

```php
/**
 * Implements hook_user_login
 */
function permissions_from_file_user_login(&$edit, $account) {

  // assign the "test role" role, if necessary.
  _permissions_from_file_update_permissions($account);

  // do not redirect password reset requests.
  if (!isset($_POST['form_id']) || $_POST['form_id'] != 'user_pass_reset') {
    if (isset($_GET['target'])) {
      drupal_goto($_GET['target']);
    }
  }
}
```

## Read File

Next we have another function that checks to see if the username is in the file or not:

```php
/**
* Reads from file and determine whether the specified user is in the list
**/
function _permissions_from_file_is_role($account) {
  $contents = file_get_contents('/var/usernames/roleusernames.csv');
  return preg_match("/$account->name/",$contents); 
  // returns if name is in the file.
}
```

## Assign or Remove Role

Finally, we can assign permissions based on whether or not their username is in the file.

```php
/**
* Calls function that checks if permissions should be found
**/
function _permissions_from_file_update_permissions($account) {
  $is_role = _permissions_from_file_is_role($account);
  $role = user_role_load_by_name('test role');
  if ($is_role) {
    // add permissions to that user
    user_multiple_role_edit(array($account->uid), 'add_role', $role->rid);
  }
  else {
    // delete the role assignment, if it exists
    user_multiple_role_edit(array($account->uid), 'remove_role', $role->rid);
  }
}
```
