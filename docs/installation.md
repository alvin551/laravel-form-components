---
title: Installation
sort: 3
---

## Notice

Most of the components in this package are based on examples from Tailwind UI. I have my own license for Tailwind UI, but you might not have one.
**Please do not use this package in your own projects without purchasing a Tailwind UI license**, or they'll have to tighten up the licensing
and you'll ruin the fun for everyone.

Purchase a license here: [https://tailwindui.com/](https://tailwindui.com)

## Installation

You can install the package via composer:

```bash
composer require rawilk/laravel-form-components
```

## Configuration

You may publish the config file via:

```bash
php artisan fc:publish
```

[Click here](https://github.com/rawilk/laravel-form-components/blob/master/config/form-components.php) to view the default configuration.

## Directives

One of the biggest advantages to `laravel-form-components` is that almost all of its components come ready
out-of-the-box. To achieve this, Laravel Form Components makes use of CDNs from
any 3rd party library that a component might need. To make sure these CDNs are
included in your HTML, you can make use of the `@fcStyles` and `@fcScripts` directives.

Place the `@fcStyles` in the `<head>` before any of your other styles. Place the `@fcScripts` directive right before
your closing `</body>` tag and **after** scripts from libraries like Livewire.

**Note:** The `@fcStyles` directives **does not** include styles generated by Laravel Form Components. You will still need to include
those in [a stylesheet manually](#styling).

If you would like to only link to the JavaScript necessary for some package components, such as the custom select component, you may
use the `@fcJavaScript` blade directive instead of `@fcScripts`. This is useful if you are compiling any third-party scripts
yourself, and is recommended in production. You should not use both directives together regardless of how you choose
to compile scripts.

## Production

Even though these directives allow you to get up and running with the components quickly, **we recommend that you compile the 3rd
party libraries that each component needs through an asset pipeline** (like [Laravel Mix](https://github.com/JeffreyWay/laravel-mix)).
By default, when your `app.debug` config option is disabled, the directives are also disabled for performance reasons.

If you always want these directives to be executed, even when `app.debug` is disabled, you can force them to load the CDN
links by passing a `true` boolean:

```html
@fcStyles(true)
@fcScripts(true)
```

Libraries are only loaded for components that are enabled through the `components` config option. You can learn more about
[disabling specific components](#components) below.

## Views

You may override the views, either by using your own views and specifying them in the config, or by publishing the package's views:

```bash
php artisan fc:publish --views
```

**Note:** This will also publish the package's configuration as well.

## Styling

Laravel Form Components includes some utility classes styled in `.scss` stylesheets. If you're using
sass, you can pull in the package's styles into your stylesheets by importing the `form-components.scss` stylesheet.

If you have a `./resources/sass/app.scss` stylesheet, you can do:

```css
@import '../../vendor/rawilk/laravel-form-components/resources/sass/form-components';

/* Add your overrides here */
```

## Components

Even though all components come enabled out-of-the-box, you might just want to
only load the components you need in your app for performance reasons. To do so,
first [publish the config file](#configuration), then remove the components
you don't need from the `components` settings.

You can also choose to use different classes and views for components. This allows you
to either extend or completely change the component's functionality by using a custom class
and/or view of your own.

## Component JavaScript

Some components, such as the [custom select component](/docs/laravel-form-components/v1/components/custom-select), require custom
JavaScript to run. The JavaScript is extracted to an external file since it is pretty substantial and should be minified. If
you are using any components that depend on this JavaScript, be sure you are pulling the scripts in through either the
`@fcJavaScript` or `@fcScripts` blade directives in your layout file. See [directives](#directives) for more information.

### Asset URL
For cases where your app's root domain is not the correct path for retrieving assets, you may set a custom root path that
FormComponents will use to link to its JavaScript assets. You may specify a custom root path either in the config file,
or as a parameter option through the blade directive:

In the config:

```php
[
    // ...
    'asset_url' => 'myapp.com/app',
];
```

In your layouts:

```html
@fcJavaScript(['asset_url' => 'myapp.com/app'])
<!-- or -->
@fcScripts(false, ['asset_url' => 'myapp.com/app'])
```

## Prefixing

Using components from this library might conflict with other ones from a different
library or components from our own app. To prevent this you can opt to prefixing
FormComponents components by default to prevent these collisions. You can do this by
setting the `prefix` option in the config file:

```php
<?php

return [
    // ...
    'prefix' => 'tw',
    // ...
];
```

Now all components can be referenced as usual but with the prefix before their name:

```html
<x-tw-form action="http://example.com" />
```

For obvious reasons, the docs don't use any prefix in their code examples. So keep
this in mind when setting a prefix and copying/pasting code snippets.