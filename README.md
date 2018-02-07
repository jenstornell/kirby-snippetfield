# Kirby Snippetfield

*Version 0.2*

This field work exactly like the [structure field](https://getkirby.com/docs/cheatsheet/panel-fields/structure). In fact it's a copy of it with some important changes.

***The Snippetfield does not use entry. Instead it uses snippets.***

## Installation

Use one of the alternatives below.

### 1. Kirby CLI

If you are using the [Kirby CLI](https://github.com/getkirby/cli) you can install this plugin by running the following commands in your shell:

```text
$ cd path/to/kirby
$ kirby plugin:install jenstornell/kirby-snippetfield
```

### 2. Clone or download

1. [Clone](https://github.com/jenstornell/kirby-snippetfield.git) or [download](https://github.com/jenstornell/kirby-snippetfield/archive/master.zip)  this repository.
2. Unzip the archive if needed and rename the folder to `kirby-snippetfield`.

**Make sure that the plugin folder structure looks like this:**

```text
site/plugins/kirby-snippetfield/
```

### 3. Git Submodule

If you know your way around Git, you can download this plugin as a submodule:

```text
$ cd path/to/kirby
$ git submodule add https://github.com/jenstornell/kirby-snippetfield site/plugins/kirby-snippetfield
```

## 1. Blueprint with `style: items` (default)

I used the example from the docs. I have replaced `entry` with `snippet` and added `style: items` (just to make it clear).

```yaml
fields:
  addresses:
    label: Addresses
    type: snippetfield
    snippet: mydir/snippet
    style: items
    fields:
      street:
        label: Street
        type: text
      zip:
        label: ZIP
        type: text
      city:
        label: City
        type: text
```

The example above will use the snippet `site/snippets/mydir/snippet.php`. You can change the path in the `config.php`.

## 1. Blueprint with `style: table`

For the table style you need a snippet for every field.

```yaml
fields:
  addresses:
    label: Addresses
    type: snippetfield
    style: table
    fields:
      street:
        label: Street
        type: text
        snippet: mydir/snippet1
      zip:
        label: ZIP
        type: text
        snippet: mydir/snippet2
      city:
        label: City
        type: text
        snippet: mydir/snippet2
```

### Config

If you don't like the root path, you can change it in your `config.php`:

```php
c::set( 'snippetfield.path', kirby()->roots()->snippets() );
```

## 2. Snippet

### `$page` object

All the page variables are available through the `$page` object and you can use them like this:

```php
echo $page->title();
```

### `$field` object

To give you a hint of what it contains, do this (might be slow or crash):

```php
print_r( $field );
```

### `$style` string

If for some reason you need to have the style value you can use that.

## 3. Snippet with `style: items` (default)

If you use `style: items` you also have access to the `$values` object.

### `$values` object

Because the output contains more than one field, all the field keys and values are in the `$values` object.

```php
print_r( $values );
```

## 3. Snippets with `style: table`

If you use `style: table` you also have access to the `$value` string.

### `$value` string

Because the output only contain one field, you only need a string and that is the `$value` string.

## Snippet with `style: table` example

In this case I have an `image` field and the `$value` will be `filename.jpg`. It is actually using the thumbnail cache to generate and store a thumbnail of an image.

```php
<img src="<?php echo thumb($page->image($value), array('width' => 150))->url(); ?>">
```

You now also have access to `$key` which is the column slug.

## What is wrong with entry?

- It does not support any logic.
- Images can not be viewed as images, only as file names.

## What can be done with Snippetfield?

- **Use calculations and logic**
- **Format values**
- **Image galleries** can be created quite easily.
- **If statements**. Maybe you want to display a pink elephant every time a value is true. Now you can.
- **Advanced stuff** could be made, like take the value, run it trough Google Analytics, get some data back and present that.

## Changelog

**0.2**

- Made it a plugin instead of a field. Will probably require Kirby 2.4.1 from now on.
- Added `$key` to `style: table` which is the column slug.
- Added `package.json` which means Kirby CLI support.

**0.1**

- Initial release
