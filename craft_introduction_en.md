# An introduction to Craft, a CMS by Pixel&Tonic

## Introduction

### Technologies and ethos

[Craft](http://buildwithcraft.com/) is a very capable CMS created by [Pixel & Tonic](http://pixelandtonic.com/) and developed using open source technologies like PHP and MySQL. It is based on a PHP framework with a great track record: [Yii](http://www.yiiframework.com/).

Craft focuses on the essentials: defining and managing your content in the most modular and flexible way possible. For example, localisation is built in from the get go but, if you need comments, a forum or e-commerce, you will have to turn to plugins.

### A modular pricing model

This flexibility is present into the [pricing model](https://craftcms.com/pricing) of the product itself.

- The "Solo" version of Craft is free. You can enjoy all functionalities of Craft except for its user management and permissions. The only account available is an admin account.
- The "Pro" version costs 299 USD and offers you full user management, a GraphQL API, the possibility to brand the system and a few other niceties.

#### Licenses

Craft licenses are perpetual, meaning you can use the version you purchased indefinitely. You also get one year of Developer support and updates bundled with your purchase.

If you want updates and support after that, you will have to pay 59 USD for one year of upgrades and support. You can pay that license fee whenever you need it.

#### Trial versions

[You can test Craft Pro and all its functionalities](https://craftcms.com/guides/try-craft-pro-plugins-before-buying) for an indefinite amount of time provided that you are on a local or staging domain that can be identified as such.

### A fully integrated e-commerce platform

Pixel & Tonic released a first party e-commerce module for Craft dubbed [Craft Commerce](https://craftcms.com/commerce). A one-time license of Craft Commerce for one site is 999 USD. For that price, you get [a ton of features](https://craftcms.com/features) and everything you need to create and manage an online store.

A "Lite" version of Commerce is also available with much simpler e-commerce capabilities for a price of 199 USD per project.

There is also a [first-party free Shopify plugin](https://plugins.craftcms.com/shopify) to integrate Craft with your Shopify products.

Covering Craft Commerce is not within the scope of this introduction to Craft. Craft Commerce shares a lot of characteristics with Craft core as far as data structure creation and templating are concerned so you won't feel lost. If you need tight e-commerce integration with your CMS, Craft Commerce is definitely an option to consider.

### Strengths

In my opinion, the main strengths of Craft are:

1. An unparalleled flexibility in the definition of your item types and of their data structure. The field types available by default allow for an extremely modular approach of your content.
2. Speaking of modular fields, Matrix is one of Craft's assets in terms of creating a flexible data structure while maintaining total control over the generated front-end code.
3. Using [Twig as its templating language](http://twig.sensiolabs.org/doc/templates.html). Learning Twig certainly has a learning curve but the benefits in terms of power, performance, modularity and flexibility for your templates are amazing.
4. An integrated and comprehensive solution for multilingual sites with great localisation features.
5. An impressive amount of features available out of the box, designed to make your life and the life of your users easier: live preview, responsive control panel, one click updates, multiple environments configurations, etc.
6. Freedom to build your own URL structure: there is almost no relation between your URL structure and your folder and files structure, thanks to a very flexible routing mechanism.
7. A fantastic development and support team.

## 1. Installation and configuration

### Setting things up

After checking your server has everything it needs to run Craft, all you have to do is [follow the installation procedure](https://craftcms.com/docs/4.x/installation.html) to install Craft and check your permissions. This should hopefully go smoothly.

### Version control

Any developer worth its salt is version controlling her/his work using Git these days. Craft has its own [`.gitignore`](https://github.com/craftcms/craft/blob/main/.gitignore) file. You have to modify it to take care of your files. This is the .gitignore file I generally use as a starting point:

```
# Standard Craft stuff
# ----------------------
.env
vendor/
.DS_Store

# uploaded resources
# ----------------------
web/uploads/**/*

# dist files
# ----------------------
web/dist/**/*

# node modules
# ----------------------
node_modules/
```

### Multiple environments configurations

Because each developer likely uses different files and folders architectures locally and because sensitives informations like database credentials should not be committed to a repository, Craft allows you to use a `.env` file at the root of your project to allow each developer to use her/his own settings. You can then use values specified in that `.env` files in `craft/config/general.php` and `craft/config/db.php` (should you decide to create that last file).

Variables prefixed with "CRAFT\_" are system variables that are automatically used by the system without needing to appear in config files.

Here is an exemple of what it looks like:

**Example**: `.env`

```
# The environment Craft is currently running in (dev, staging, production, etc.).
CRAFT_ENVIRONMENT=dev

# The application ID used to to uniquely store session and cache data, mutex locks, and more
CRAFT_APP_ID=randomid

# The secure key Craft will use for hashing and encrypting data
CRAFT_SECURITY_KEY=randomkey

# Database Configuration
CRAFT_DB_DRIVER=mysql
CRAFT_DB_SERVER=127.0.0.1
CRAFT_DB_PORT=3306
CRAFT_DB_DATABASE=dbname
CRAFT_DB_USER=dbuser
CRAFT_DB_PASSWORD=dbpassword
CRAFT_DB_SCHEMA=public
CRAFT_DB_TABLE_PREFIX=

# General configuration
DEV_MODE=true
ALLOW_ADMIN_CHANGES=true
DISALLOW_ROBOTS=true

# The URI segment that tells Craft to load the control panel
CP_TRIGGER=admin

# Base URL and path (no trailing slashes)
BASE_URL = https://myproject.craft.test
BASE_PATH = /Users/username/data/weblocal/myproject
```

You will then be able to use all values defined in that `.env` files in your configuration files `craft/config/general.php` et `craft/config/db.php` (you will have to create that file if it does not exists). Those files allow you to define all of Craft's [configuration settings](https://craftcms.com/docs/4.x/config/config-settings.html).

You can also use these values to create [Yii aliases](https://craftcms.com/docs/4.x/config/#aliases) that you can use in the contyrol panel, for example to define the paths and URLs of your assets FileSystems. You can also use those in your templates with the `alias()` function.

#### Map or Fluent

Your settings files can use two syntaxes; map of fluent. The main advantages of the "Fluent" syntax are autocompletion and inline documentation.

**Example (map)**: `config/general.php`

```php
<?php
/**
 * General Configuration
 *
 * All of your system's general configuration settings go in here. You can see a
 * list of the available settings in vendor/craftcms/cms/src/config/GeneralConfig.php.
 *
 * @see \craft\config\GeneralConfig
 */

use craft\helpers\App;

$isDev = App::env('CRAFT_ENVIRONMENT') === 'dev';
$isProd = App::env('CRAFT_ENVIRONMENT') === 'production';

return [
  'defaultWeekStartDay' => 1,
  'omitScriptNameInUrls' => true,
  'limitAutoSlugsToAscii' => true,
  'defaultSearchTermOptions' => [
    'subLeft' => true,
    'subRight' => true,
  ],
  'devMode' => $isDev,
  'allowAdminChanges' => $isDev,
  'disallowRobots' => !$isProd,
  'securityKey' => App::env('SECURITY_KEY'),
  'cpTrigger' => App::env('CP_TRIGGER') ?: 'admin',
  'aliases' => [
    '@environment' => App::env('CRAFT_ENVIRONMENT'),
    '@web' => App::env('BASE_URL'),
    '@baseUrl' => App::env('BASE_URL'),
    '@basePath' => App::env('BASE_PATH'),
    '@assetsBasePath' => App::env('BASE_PATH').'/uploads',
    '@assetsBaseUrl' => App::env('BASE_URL').'/uploads',
    '@webroot' => App::env('BASE_PATH')
  ]
];
```

**Example (fluent)**: `config/general.php`

```php
<?php
/**
 * General Configuration
 *
 * All of your system's general configuration settings go in here. You can see a
 * list of the available settings in vendor/craftcms/cms/src/config/GeneralConfig.php.
 *
 * @see \craft\config\GeneralConfig
 */

use craft\config\GeneralConfig;
use craft\helpers\App;

$isDev = App::env('CRAFT_ENVIRONMENT') === 'dev';
$isProd = App::env('CRAFT_ENVIRONMENT') === 'production';

return GeneralConfig::create()
  ->defaultWeekStartDay(1)
  ->omitScriptNameInUrls()
  ->limitAutoSlugsToAscii(true)
  ->defaultSearchTermOptions([
    'subLeft' => true,
    'subRight' => true,
  ])
  ->devMode($isDev)
  ->allowAdminChanges($isDev)
  ->disallowRobots(!$isProd)
  ->securityKey(App::env('SECURITY_KEY'))
  ->cpTrigger(App::env('CP_TRIGGER') ?: 'admin')
  ->aliases([
    '@environment' => App::env('CRAFT_ENVIRONMENT'),
    '@web' => App::env('BASE_URL'),
    '@baseUrl' => App::env('BASE_URL'),
    '@basePath' => App::env('BASE_PATH'),
    '@assetsBasePath' => App::env('BASE_PATH').'/uploads',
    '@assetsBaseUrl' => App::env('BASE_URL').'/uploads',
    '@webroot' => App::env('BASE_PATH')
  ])
;
```

**Example (map)**: `config/db.php`.

Il you use environement variables automatically detected by Craft to connect to your database ("CRAFT_DB_DRIVER", "CRAFT_DB_DATABASE", etc.) you likely will not need this configuration file.

```php
<?php
/**
 * Database Configuration
 *
 * All of your system's database configuration settings go in here. You can see a
 * list of the available settings in vendor/craftcms/cms/src/config/DbConfig.php.
 *
 * @see \craft\config\DbConfig
 */

use craft\helpers\App;

return [
  'driver' => App::env('DB_DRIVER'),
  'server' => App::env('DB_SERVER'),
  'user' => App::env('DB_USER'),
  'password' => App::env('DB_PASSWORD'),
  'database' => App::env('DB_DATABASE'),
  'schema' => App::env('DB_SCHEMA'),
  'tablePrefix' => App::env('DB_TABLE_PREFIX'),
  'port' => App::env('DB_PORT')
]
```

**Example (fluent)**: `config/db.php`.

Il you use environement variables automatically detected by Craft to connect to your database ("CRAFT_DB_DRIVER", "CRAFT_DB_DATABASE", etc.) you likely will not need this configuration file.

```php
<?php
/**
 * Database Configuration
 *
 * All of your system's database configuration settings go in here. You can see a
 * list of the available settings in vendor/craftcms/cms/src/config/DbConfig.php.
 *
 * @see \craft\config\DbConfig
 */

use craft\config\DbConfig;
use craft\helpers\App;

return DbConfig::create()
  ->driver(App::env('DB_DRIVER'))
  ->server(App::env('DB_SERVER'))
  ->port(App::env('DB_PORT'))
  ->database(App::env('DB_DATABASE'))
  ->user(App::env('DB_USER'))
  ->password(App::env('DB_PASSWORD'))
  ->schema(App::env('DB_SCHEMA'))
  ->tablePrefix(App::env('DB_TABLE_PREFIX'))
;
```

**Example**: aliases usage in the control panel for assets (local) volumes definition.

```
@assetsBasePath/partners_logos/
@assetsBaseUrl/partners_logos/
```

**Example**: aliases usage in templates with the `alias()` function.

```twig
{% if alias('@environment') == 'production' %}
  {# include analytics code #}
{% endif %}
```

```twig
<link rel="stylesheet" href="{{ alias('@baseUrl') }}/dist/css/main.min.css">
```

Values defined via `dotenv` and used in your production configuration must be available to Craft in your production environment. That is usually done directly in your web server configuration, be it Apache or Nginx. `.env` file are (generally) not used in production and hosting providers will offer you a way to configure environment variables at the server level.

### WYSIWYG Editors (HTML)

Craft offers various plugins that can be used as WYSIWYG solutions if you want your users to be able to enter rich text.

I personally use [CKEditor](https://plugins.craftcms.com/ckeditor) with pretty simple configurations. It is available from the plgin store as a free first-party plugin developped by Pixel&Tonic

CKEditor configurations can be created using the ontrol panel once the plugin is installed.

## 2. Create your data structure

Craft allows you to create very flexible and modular data structures for your projects.

### Sections, Entries and entry types

Since Craft 4.4 the plan is to replace tags, categories and globals with sections and entries.

With Craft, your content will mainly live in entries. Those entries are grouped under entry types to form sections.

The data structure of those entries is created by assigning custom fields to the entry types you defined for each sections. For each of those entry types, you can create a field layout defining which fields will be used by all entries of that type in a given section.

There are [three types of sections](https://craftcms.com/docs/4.x/entries.html): singles, channels and structures.

#### Single sections

These sections contain only one entry type and only one entry. They are used to create one off pages in your site, like an "about page" or a "homepage".

They can also be used to hold content of a more global nature, for example URLs of your social media accounts, global site configuration options, etc.

Using the section configuration screen you can define:

- The URL format for the entry.
- The template Craft should load to display that entry.
- For global content, you might not need to specify URLs or templates.

The field layout screen allows you to assign custom fields to your entry type in order to define the data structure of your entry.

#### Channel sections

Think of those sections as streams of entries that can be filtered, ordered and displayed in various ways using your templates (news, case studies, press releases, pictures, videos, etc.).

Using the section configuration screen you can define

- The URL format for all entries in that section.
- The template Craft should load to display entries belonging to that section.
- For some channels, typically those where entries are only displayed within other templates and that do not need a detail page, you might not need to specify a URL formats or a template.

Channel sections can contain various entry types, each having their own data structure. For example creating a blog allowing you to post various types of content is very easy. All you need is a channel section with various entry types (blogposts, links, videos, pictures, etc.)

For each entry type defined in your section, you can define the data structure of entries by assigning custom fields to a field layout.

These entry types can easily be [used in routing and URL structures](https://craftcms.com/knowledge-base/entry-type-urls) as well as in your templates with [`craft.entries` tags](https://craftcms.com/docs/4.x/entries.html#example).

#### Structure sections

Structure sections behave a lot like channel sections: they can contain various entry types and the URL structure of their entries can be specified. The main difference is that their entries can be manually ordered in a hierarchical tree. You can specify the number of levels in that hierarchy.

Using the section configuration screen you can define

- The URL format for entries in that section. Different URL pattern can be specified for top-level entries and for nested entries.
- The template Craft should load to display entries belonging to that section.
- In some cases, you might not need to specify a URL format or a template.

For each entry type defined in your section, you can define the data structure of entries by assigning custom fields to a field layout.

### Fields, Field Groups and Field Layouts

Craft comes natively with [numerous field types](https://craft-docs.netlify.app/docs/4.x/fields.html) through which you can define the data structure of your entries.

A field can be applied to several entries, users or assets via a "field layout" allowing you to perform operations on the fields (ordering, make mandatory or not, make conditional fields, etc.) via a drag and drop interface.

Fields can be grouped into field groups. These groups are there purely for convenience's sake to help you manage big projects more easily.

### Users

Craft has [powerful and granular user management](https://craft-docs.netlify.app/docs/4.x/users.html) allowing you to manage the permissions for all users of the system.

Users can be assigned to various user groups. Permissions can be managed at a user group or at a user level. A user can be assigned to multiple groups.

The data structure for your users can be defined easily. A unique field layout allows you to assign custom fields to all your users, regardless of their groups.

### Assets

[Assets](https://craftcms.com/docs/4.x/assets.html) allow you to manage your files with Craft (images, videos, sounds, PDFs, etc). Assets are assigned to Assets Volumes (handling custom fields) and to Assets and Transform Filestystems corresponding to folders on your server, or on external Cloud servers like Rackspace or Amazon S3.

Craft allows you to apply [transforms](https://craftcms.com/docs/4.x/image-transforms.html) (crop, fit, scale, quality, etc.) to images automatically, either in the control panel or dynamically through templates. That feature allows you to generate all the thumbnails you needs from an initial image.

A field layout is available for each Asset volume. Using it in combination with custom fields allow you to create complex data structure for each of your assets types. For example, documents can have a different data structure than photos.

### Relations

One of Craft's great strengths is its [relations system](https://craftcms.com/docs/4.x/relations.html). You can easily create relations between Entries, Users, Assets and Tags through a series of relational field types:

- **Assets**: allows you to establish a "one to one" or "one to many" relation to Assets.
- **Entries**: allows you to establish a "one to one" or "one to many" relation to Entries.
- **Users**: allows you to establish a "one to one" or "one to many" relation to Users.

For each of those field, you can specify how many items can be linked and from what source(s) they come from.

To display and work with these relations in your templates, Craft is giving you a very powerful tool in the form of the [`relatedTo`](https://craftcms.com/docs/4.x/relations.html#the-relatedto-parameter) parameter. You can use that parameter with `craft.entries`, `craft.users` and `craft.assets`.

### Routing

Another interesting aspect of Craft is its [dynamic routing system](https://craftcms.com/docs/4.x/routing.html), which allows you separate your URL structure from your folders and files structure.

At the most basic level, we have already seen that Craft allows you to specify the URL structure of each of your Entries, Users, Assets, Tags and Categories.

If that's not enough to cover all your needs, you can also create dynamic routes independently. For each route you create, you can specify which template must be loaded by Craft. An easy to understand example is [a template giving access to a yearly archive of entries](https://craftcms.com/guides/creating-an-archive-page-for-entries#yearly-archive-pages).

When you create a dynamic route with the structure of `blog/archive/{year}` calling the template `blog/archive`, the URLs `blog/archive/2013` and `blog/archive/2012` will load the same template and make a `{year}` variable accessible to to Twig in that template. You can for example use it with the [`after`](https://craftcms.com/docs/4.x/entries.html#after) and [`before`](https://craftcms.com/docs/4.x/entries.html#before) parameters of `craft.entries()`.

If you need more possibilities than those the Control Panel gives you, you can [manage your routes in a more advanced way](https://craftcms.com/docs/4.x/routing.html#advanced-routing-with-url-rules) using the `config/routes.php` file. That will give you access to all the power of regular expressions in your URL matching patterns.

## 3. Twig as templating language

Craft uses [Twig](http://twig.sensiolabs.org/), created by Fabien Potencier, as its templating language. Twig compiles all your templates down to raw PHP, which means it is blazing fast. Twig has a small learning curve if you have never done any programming but remains very [accessible to front-end developers](http://twig.sensiolabs.org/doc/templates.html).

Coupled with Craft specific tags, functions and filters, Twig enables you to access your data and manipulate them in your templates. For a quick introduction to Twig other than what follows, have a look at "[Twig for designers](http://www.slideshare.net/brandonkelly212/twig-for-designers)", a presentation by Brandon Kelly available on Slideshare.

### Main tags in Twig

On top of the [tags available in Twig](http://twig.sensiolabs.org/doc/tags/index.html), Craft also has a few tags of its own. We will cover some of them in the rest of this course.

Twig has three main types of tags:

- `{# Comments #}`
- `{{ Output }}`
- `{% Execution / Logic %}`

#### Comment tags

Twig comment tags are not rendered in the compiled template: `{# This is a comment: not rendered in the compiled template #}`.

#### Output tags, variables and properties

`{{ Output }}`: These tags allow you to output strings, numbers, booleans, arrays and objects in your templates. Most of the time you will be outputting variables that you or Craft sets.

Examples:

- `{{ "Hello world" }}`: displays the string "Hello world"
- `{{ entry.title }}`: displays the title property of an entry object
- `{{ 8 + 2 }}`: displays 10

#### Execution and logic tags

`{% Execution / Logic %}`: these tags allow you to execute tasks and can be used to create variables, loops, control structures, etc.

Examples:

Create an `allEntries` variable and assign it the results of an [ElementQuery](https://craftcms.com/docs/4.x/element-queries.html) containing the last 10 entries in the `blog` section, ordered by creation date in descending order.

```twig
{% set allEntries = craft.entries()
  .section('blog')
  .limit(10)
  .order('postDate desc')
  .all() %}
```

Loop on `allEntries` array of `entry` objects and display their titles.

```twig
{% for entry in allEntries %}
  <h1>{{ entry.title }}</h1>
{% endfor %}
```

Create a [control structure](http://twig.sensiolabs.org/doc/templates.html#control-structure) to check if `allEntries` contains at least one entry.

```twig
{% if allEntries|length %}
  <p>There is at least one entry here</p>
{% endif %}
```

#### Never use tags inside other tags

These are incorrect:

```twig
{{ "Hello {{ firstname }}" }}
{% set total = {{ itemPrice }} * {{ itemsNbr }} %}
```

You will have to use the following syntaxes

```twig
{{ "Hello " ~ firstname }}
{% set total = itemPrice * itemsNbr %}
```

### Data types and variables with Twig

Twig supports 5 big data types:

- Strings: `"Hello"`
- Numbers: `4` or `3.1498`
- Booleans: `true` or `false`
- Arrays: `['orange', 'lemon', 'apple']`
- Objects: `{ foo: 'bar' }`

As shown earlier, you can easily assign a value to a variable using the `{% set %}` tag

```twig
{% set firstName = "Jérôme" %}`
{% set allEntries = craft.entries()
  .section('blog')
  .order('postDate desc')
  .all() %}
```

### Filters

[Twig](http://twig.sensiolabs.org/doc/filters/index.html) and [Craft](https://craftcms.com/docs/4.x/dev/filters.html) have filters that can be applied to your variables and modify or manipulate them. It means that Craft doesn't need any plugin to perform simple tasks in your templates. Here are some examples of what can be done using filters:

Convert a string to title case.

```twig
{{ entry.title|title }}
```

Dates operations and formatting.

```twig
{{ entry.postDate|date_modify("+1 day")|date("m/d/Y") }}
```

Determine the length of a string, array or object.

```twig
{% set allEntries = craft.entries()
  .section('blog')
  .order('postDate desc')
  .all() %}
{{ allEntries|length }}
```

It's also possible to apply a filter to multiple lines of your templates.

```twig
{% filter upper %}
  This text becomes uppercase
{% endfilter %}
```

Filters can also be combined.

```twig
{{ "Hello World"|upper|slice(0,5) }}
```

### Functions

[Twig](http://twig.sensiolabs.org/doc/functions/index.html) and [Craft](https://craftcms.com/docs/4.x/dev/functions.html) both have functions allowing you to execute functions on your data.

```twig
{% for entry in allEntries %}
  <p class="{{ cycle(['odd', 'even'],loop.index0) }}">{{ entry.title }}</p>
{% endfor %}
```

```twig
{{ min([1, 2, 3]) }}
{{ max([1, 2, 3]) }}
{{ random(10) }}
{{ range[1..10] }}
```

`dump()` is only accessible in Craft when [Dev Mode is enabled](https://craftcms.com/guides/what-dev-mode-does) in your `config/general.php` file. Useful for debugging.

```twig
{{ dump(entry) }}
```

### Control structures and conditionals

Twig allows you to use control structures and conditionals.

**if**

```twig
{% if allEntries|length > 0 %}
<p>There is at least one entry here</p>
{% endif %}
```

**if/else**

```twig
{% if currentUser.admin %}
  <p>Welcome, master<p>
{% else %}
  <p>What can I do for you, dear user?</p>
{% endif %}
```

**nested if/else**

```twig
{% if currentUser %}
  {% if currentUser.admin %}
    <p>Hello Admin!</p>
  {% else %}
    <p>Hello user!</p>
  {% endif %}
{% elseif currentUser.friendlyName %}
  <p>May I call you {{ currentUser.friendlyName }}?</p>
{% else %}
  <p>Have we met?</p>
{% endif %}
```

**and & or in conditions**

```twig
{% if currentUser and allEntries|length %}
{% if superAdmin or admin %}
```

**Loop**

```twig
{% for entry in allEntries %}
  {% if loop.first %}<ul>{% endif %}
    <li>
      <article>
        <h2><a href="{{ entry.url }}">{{ entry.title }}</a></h2>
        {{ entry.summary }}
      </article>
    </li>
  {% if loop.last %}</ul>{% endif %}
{% else %}
  <p>No entries found</p>
{% endfor %}
```

### Mathematical expressions and string manipulations

Twig knows [Math](http://twig.sensiolabs.org/doc/templates.html#math) and allows you to manipulate character strings easily using [filters](http://twig.sensiolabs.org/doc/filters/index.html).

```twig
{{ 10 * (8+2) }}
{{ "Hello World"|slice(0,5) }}
```

### Whitespace control

Twig allows you to control how your code is displayed, including [at the whitespace level](http://twig.sensiolabs.org/doc/templates.html#whitespace-control).

### Template inheritance, includes and macros: stay DRY

These three concepts are at the heart of Twig as a templating language. They enable you to create efficient templates and avoid redundancy, allowing you to comply with the infamous "Don't Repeat Yourself" (DRY) principle.

#### Template inheritance

Template inheritance is a central concept in Twig, as it is in many other templating languages for the web. We will generally work with a "parent" template defining the skeleton of the page, its different blocks and one or more "children" templates. These children templates will extend the parent template and define the content of each of those blocks.

Variables defined in "children" templates can be accessed in the "parent" template.

Let's see how it works practically with a very simple example:

**Parent template: layouts/\_base.html**

```twig
{% set metaTitle = metaTitle|default("Default title") %}
{% set metaDescription = metaDescription|default("Default description") %}
{% set metaImage = metaImage|default(alias("@baseUrl/dist/img/socialmedia.jpg")) %}

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>{{ metaTitle }} - My site name</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="alias('@baseUrl/assets/css/styles.css')">
</head>
<body>
  {% block content %}
    <p>Content block overridden by content of child template</p>
  {% endblock %}
</body>
</html>
```

**Child template: news/index.html**

```twig
{% extends "layouts/_base.html" %}
{% set metaTitle = "Latest News" %}
{% set metaDescription = "Everything we’ve been up to lately" %}

{% block content %}

  <h1>News</h1>

  {% set allNews = craft.entries()
    .section('news')
    .orderBy("postDate DESC")
    .all() %}
  {% for entry in allNews %}
    <article>
      <h3><a href="{{ entry.url }}">{{ entry.title }}</a></h3>
      <p>Posted on {{ entry.postDate.format('F d, Y') }}</p>
      {{ entry.body }}
      <p><a href="{{ entry.url }}">Continue reading</a></p>
    </article>
  {% endfor %}

{% endblock %}
```

The `news/index.html` template extends the base layout template. The content defined in the `content` block of the child template replaces / supersedes the content of the `content` block in the parent template. The `htmlTitle` variable defined in the child template is accesible to the parent template.

As a side note, the name of the "parent" template begins with an underscore to tell Craft that template is hidden and cannot be accessed directly via a web browser.

Typically, your layouts, includes and entry templates should all be hidden.

#### Includes

If you have code that is repeated in many templates, it is generally a good idea to use includes `{% include %}`. Since Twig compiles everything down to raw PHP, there is no real performance penalty for using includes.

```twig
{% include '_sidebars/default.html' %}
```

#### Macros

Twig [Macros](http://twig.sensiolabs.org/doc/tags/macro.html) are comparable to mixins in Sass. They essentially are small reusable chunks of code.

A Macro is defined using the `{% macro %}` and `{% endmacro %}` tags. Macro can be loaded from an external file, or reside in the file they are used in.

```twig
{% macro dateText(date) %}
  {{ date|date('F j, Y') }}
{% endmacro %}

{% macro dateNumeric(date) %}
  {{ date|date('d.m.Y') }}
{% endmacro %}
```

Macros are imported using the `{% import %}` tag

```twig
{% import "_macros/dates" as dateHelpers %}
{{ dateHelpers.dateText(entry.postDate) }}
```

## 4. Retrieve, manipulate and display your data

In Craft, you interact with the database using [Element Queries](https://craftcms.com/docs/4.x/element-queries.html). It sounds complicated but it is in fact quite simple:

1. you create an ElementQuery for the type of data you want to get from the database (entries, users, assets, etc.)
2. you specify the parameters (limit, order, filters, etc.) you want to use.
3. Your execute the ElementQuery by using the following function: `.all()`, `one()`, `exists()`, `.count()` or `.ids()`
4. Craft returns an element or an array of elements objects ([entry](https://docs.craftcms.com/api/v4/craft-elements-entry.html), [user](https://docs.craftcms.com/api/v4/craft-elements-user.html), [asset](https://docs.craftcms.com/api/v4/craft-elements-asset.html).
5. You can then display those objects or arrays of objects in your template.

`craft.entries()`, `craft.users()` and `craft.assets()` will be your main tools to retrieve and display your data.

We will mainly work with `craft.entries()` in this introduction. Since all tags use the same principles, it will be easy for you to apply what you know to `craft.users()` and `craft.assets()`.

### Entries

[`craft.entries()`](https://craftcms.com/docs/4.x/entries.html#querying-entries) is going to be your main tool to retrieve and display your entries.

- `craft.entries().all()` allows you to retrieve all entries corresponding to the specified criteria.
- `craft.entries().one()` allows you to retrieve the first entry corresponding to the specified criteria (returns `null` if no entry corresponds). If you want the last entry, you can use `craft.entries().inReverse().one()`
- `craft.entries().exists()` allows you to see if there is at least one entry corresponding to the specified criteria (returns `true` or `false`).
- `craft.entries().count()` allows you to retrieve the total number of entries corresponding to the specified criteria.
- `craft.entries().ids()` allows you to retrieve the ids of all entries corresponding to the specified criteria.

Since Craft 4, we can also use `.collect()` to return elements as Laravel Collections. These have several uses:

1. Simplify template code when eager-loading elements: we can use the (nicer) dot notation instead of having to switch to arrays when eager-loading.
2. Give us access to quite a few [useful manipulation methods](https://laravel.com/docs/9.x/collections#available-methods) from Laravel.

#### A very simple element query

Here is a simple example of an entry element query.

```twig
{% set recentNews = craft.entries()
  .section('news')
  .orderBy('postDate DESC')
  .limit(4)
  .all() %}

{% for entry in recentNews %}

  <h2>{{ entry.title }}</h2>
  {{ entry.summary }}

{% endfor %}
```

You can also build much more complex Element Queries using [Advanced Element Queries](https://craftcms.com/docs/4.x/element-queries.html#advanced-element-queries).

#### No results

You can easily display alternate content if no entries are found, just by using [an `{% else %}` clause in your `{% for %}` loop](http://twig.sensiolabs.org/doc/tags/for.html#the-else-clause).

```twig
{% set recentNews = craft.entries()
  .section('news')
  .orderBy('postDate DESC')
  .limit(4)
  .all() %}

{% for entry in recentNews %}

  <h2>{{ entry.title }}</h2>
  {{ entry.summary }}

{% else %}

  <p>No news found.</p>

{% endfor %}
```

#### "Loop", "cycle" and "is divisible by"

When using a `{% for %}` loop, it is sometimes useful to know at which iteration of the loop you are currently at. Typical tests include opening and closing a `<ul>` at the beginning and at the end of a loop, or displaying something every x iterations. Twig gives you the [`loop`](http://twig.sensiolabs.org/doc/tags/for.html#the-loop-variable) variable, the [`cycle`](http://twig.sensiolabs.org/doc/functions/cycle.html) function and the [`is divisibleby`](http://twig.sensiolabs.org/doc/tests/divisibleby.html) test for those jobs.

**Example**: using the `loop` variable

```twig
{% set recentNews = craft.entries()
  .section('news')
  .orderBy('postDate DESC')
  .limit(4)
  .all() %}

{% for entry in recentNews %}
  {% if loop.first %}<ul>{% endif %}

    <li>
      <h2>{{ entry.title }}</h2>
      {{ entry.summary }}
    </li>

  {% if loop.last %}</ul>{% endif %}
{% endfor %}
```

**Example**: using `cycle` to add `odd` and `even` classes in your html. `loop.index0` is used to have a zero indexed iteration rather than the 1 based iteration that `loop.index` gives you.

```twig
{% set recentNews = craft.entries()
  .section('news')
  .orderBy('postDate DESC')
  .limit(4)
  .all() %}

{% for entry in recentNews %}
  {% if loop.first %}<ul>{% endif %}

    <li class="{{ cycle(['odd', 'even'], loop.index0) }}">
      <h2>{{ entry.title }}</h2>
      {{ entry.summary }}
    </li>

  {% if loop.last %}</ul>{% endif %}
{% endfor %}
```

[Other Twig tests](http://twig.sensiolabs.org/doc/tests/index.html) might prove useful, most notably the `is defined` test. The other tests available in Twig are:

- `is constant`
- `is defined`
- `is divisible by`
- `is empty`
- `is even`
- `is iterable`
- `is null`
- `is odd`
- `is same as`

#### Pagination

Craft allows you to [paginate your results](https://craftcms.com/docs/4.x/dev/tags.html#paginate) using the `{% paginate %}` tag and to build simple or more complex pagination interfaces using the related variables. One small caveat: the `{% paginate %}` tag needs an ElementQuery as parameter. Just don't call `all()` on it in this case.

```twig
{% set allNews = craft.entries()
  .section('news')
  .orderBy('pubDate DESC') %}

{% paginate allNews as paginate, entries %}

{# get paginated entries #}
{% for entry in entries %}
  <article>
    <h3 class="title-item"><a href="{{ entry.url }}">{{ entry.title }}</a></h3>
    <p class="meta-info">Posted on {{ entry.postDate.format('F d, Y') }}</p>
    {{ entry.body }}
    <p><a href="{{ entry.url }}">Read More</a></p>
  </article>
{% endfor %}

{# Build pagination interface if more than 1 page #}
{% if paginate.totalPages > 1 %}
  <ul class="hlist pagination">
    {% if paginate.prevUrl %}
      <li><a href="{{ paginate.prevUrl }}">Previous Page</a></li>
    {% endif %}

    {% for page, url in paginate.getPrevUrls(2) %}
        <li><a href="{{ url }}">{{ page }}</a></li>
    {% endfor %}

    <li class="current"><a href="{{ paginate.getPageUrl( paginate.currentPage ) }}">{{ paginate.currentPage }}</a></li>

    {% for page, url in paginate.getNextUrls(2) %}
        <li><a href="{{ url }}">{{ page }}</a></li>
    {% endfor %}

    {% if paginate.nextUrl %}
      <li><a href="{{ paginate.nextUrl }}">Next Page</a></li>
    {% endif %}

  </ul>
{% endif %}
```

#### Detail page and "entry" variable

When Craft is loading an URL corresponding to an entry URL you specified when creating your sections, the system will automatically populate an `entry` variable and make it accessible in our template. Thanks to that `entry` variable and to Craft's routing system, you don't need anything else to display your entry in a detail page.

```twig
{#
 # This template gets loaded whenever a News entry’s URL is
 # requested. That’s because the News section’s Template setting is
 # set to “news/_entry”, the path to this template.
 #
 # When this template is loaded, it will already have an ‘entry’
 # variable, set to the requested News entry.
 #}

{% extends "layouts/_base.html" %}

{% block content %}

  <article>
    <h1>{{ entry.title }}</h1>
    <p>Posted on {{ entry.postDate.format('F d, Y') }}</p>
    {{ entry.body }}
  </article>

{% endblock %}
```

#### Category page

Working solely on the basis of a generated "entry" variable can be problematic if two sections are refrencing the same template.

Let's take a concrete example with a `templates/news/index.twig` template that has to display a title, an introduction a list of news and a list of news topics.

- We have a single section with an URL of `news` referencing that template to display the title and the intro
- We have a structure section for topics with an url of `news/{slug}` referencing that template to display the list of topics and to filter the list of news if a topic is selected.

When the route is `news` the `entry` variable defined by Craft references the single entry, but when the route has a format of `news/{slug}`, that `entry` variable corresponds to an entry from the structure section.

The simplest way to go about it is to take the matter into our own hands and to define everything explicitely ourselves using Twig.

```twig
{#
 # This template gets loaded by two routes / URL,
 # which means we cannot rely on craft automatically creating an `entry` variable,
 # because the content of this variable will change depending on the route / URL
 #
 # - when the route is `news/` the `entry` variable references the entry from the single section
 # - when the route is `news/{slug}`, the `entry` variable references one of the entries from the structure section
 #}

{# layout used #}
{% extends "layouts/_base.html" %}

{% block content %}

  {# Define an entry variable by hand, the id is the one of the entry from the single section #}
  {% set entry = craft.entries().id(7).one() %}

  {# Define a variable for our category based on the slug in the URL #}
  {% set categorySlug = craft.app.request.getSegment(2) ?? null %}
  {% set category = categorySlug ? craft.entries().section("newsCategories").slug(categorySlug).one() : "all" %}

  {# get all news IDs (used to only display categories with active entries) #}
  {% set allNewsIds = craft.entries()
    .section("news")
    .ids() %}

  {# get all our active categories (categories with at least one entry) #}
  {% set allCategories = craft.entries()
    .section("newsTopics")
    .relatedTo(allNewsIds)
    .all() %}

  {# get all our news from the news channel, do not use all() or collect() because we need to paginate those #}
  {% set allNews = craft.entries()
    .section("news")
    .limit(10) %}

  {# if there is a category, add relatedTo parameter to news query using "do" so we don't output anything #}
  {% if category != "all" %}
    {% do allNews.relatedTo(category) %}
  {% endif %}

  {# display page title using entry variable #}
  {{ entry.pageTitle }}

  {# display news list #}
  {% paginate allNews as pagination, news %}

  {% for item in news %}
    {% if loop.first %}<ul>{% endif %}

      <article class="newscard">
        <p class="newscard__meta"><time datetime="{{ item.postDate|date("Y-m-d") }}">{{ item.postDate|date("F j, Y") }}</time></p>
        <h2 class="newscard__title"><a href="{{ item.url }}">{{ item.title }}</a></h2>
        <p class="newscard__summary">{{ item.summary }}</p>
      </article>

    {% if loop.last %}</ul>{% endif %}
  {% else %}
    <p>No news found</p>
  {% endfor %}

  {# Build pagination interface if more than 1 page #}
  {% if pagination.totalPages > 1 %}
    <ul class="hlist pagination">
      {% if pagination.prevUrl %}
        <li><a href="{{ pagination.prevUrl }}">Previous Page</a></li>
      {% endif %}

      {% for page, url in pagination.getPrevUrls(2) %}
          <li><a href="{{ url }}">{{ page }}</a></li>
      {% endfor %}

        <li class="current"><a href="{{ pagination.getPageUrl( pagination.currentPage ) }}">{{ pagination.currentPage }}</a></li>

      {% for page, url in pagination.getNextUrls(2) %}
          <li><a href="{{ url }}">{{ page }}</a></li>
      {% endfor %}

      {% if pagination.nextUrl %}
        <li><a href="{{ pagination.nextUrl }}">Next Page</a></li>
      {% endif %}

    </ul>
  {% endif %}

  {# display categories list #}
  {% for item in allCategories %}
    {% if loop.first %}
      <ul>
      <li><a href="{{ siteUrl }}news/"{% if category == "all" %} class="current"{% endif %}>All Categories</a></li>
    {% endif %}

      <li><a href="{{ category.url }}"{% if currentCategory == category.slug %} class="current"{% endif %}>{{ category.title }}</a></li>

    {% if loop.last %}</ul>{% endif %}
  {% endfor %}

{% endblock %}
```

#### Single sections and global values

Single sections can also be used to store global informations (company info, social media, etc.).

If you have a single with the handle of `companyInfo` with a `compayName` field, you have two solutions:

- Check that `preloadSingles()` is activated in your `config/general.php` file and access the field directly `{{ companyInfo.companyName }}`
- Use standard Craft and Twig tags

```twig
{% set companyInfo = craft.entries().section("companyInfo").one() %}
{{ companyInfo.tagline }}
```

### Users

`craft.users()` allows you to access and display users elements and has [a few parameters](https://craftcms.com/docs/4.x/users.html) that are tied to users-specific functionalities or behaviors.

User queries function like `craft.entries()` but returns a single [`User`](https://docs.craftcms.com/api/v4/craft-elements-user.html) element or an array of those.

### Assets and transforms

`craft.assets()` allows you to access your assets. This tag also has a series of [parameters](https://craftcms.com/docs/4.x/assets.html#parameters), some of which are tied to assets-specific functionalities or behavior.

`craft.assets` works like `craft.entries()` except that it returns a single [`Asset`](https://docs.craftcms.com/api/v4/craft-elements-asset.html) element or an array of those.

If your assets are images, Craft allows you to create transforms tied to all your asset groups. Transforms will generate thumbnails for all your assets. Transforms can be specified in the control panel and generated when assets are uploaded (Settings > Assets > Image Transforms) or they can be specified in your template and generated dynamically when assets are requested for the first time.

When you define a transform in the Control Panel and you name it `thumbnail`, you can access them in your templates in the following way:

```twig
{% set heroImage = entry.myAssetField.one() %}
{% if heroImage %}
  <img src="{{ heroImage.getUrl('thumbnail') }} width="{{ asset.getWidth('thumbnail') }}" height="{{ asset.getHeight('thumbnail') }}" alt="{{ heroImage.title }}">
{% endif %}
```

You can also specify your transforms directly in your templates, which is a more centralised way of doing it.

```twig
{% if myAssetField | length %}
  {% set image = myAssetField.one() %}
  <img src="{{ image.getUrl({ width: 100, height: 100 }) }}
       width="100"
       height="100"
       alt="{{ image.alt }}">
{% endif %}
```

or

```twig
{% set smallSquareThumb = {
    width: 100,
    height: 100,
    mode: 'crop',
    position: 'center-center'
} %}

{% if myAssetField | length %}
  {% set image = myAssetField.one() %}
  <img src="{{ image.getUrl(smallSquareThumb) }}
       width="{{ image.getWidth(smallSquareThumb) }}"
       height="{{ image.getHeight(smallSquareThumb) }}"
       alt="{{ image.alt }}">
{% endif %}
```

### Matrix

[Matrix](https://craftcms.com/features/all#matrix) certainly is one of the most interesting field type available with Craft. Essentially, it allows you to define a data structure for several types of content blocks (using Craft's other field types) and then to combine and arrange these blocks of content.

For example, you could create a `modularBody` Matrix field with the following configuration:

- `textBlock` block type
  - `mxTextTxt`rich text field (CKEditor)
- `quoteBlock` block type
  - `mxQuoteTxt` Textfied (256)
  - `mxQuoteAuthor` Textfied (128)
- `imageBlock`block type
  - `mxImageFile` Asset file (limited to 1)
  - `mxImageCaption` Textfied (128)
  - `mxImageCopyright` Textfied (128)
  - `mxImageFullwidth` Lightswitch field

Such a Matrix field would allow your users to combine and arrange those text, image and quote blocks when creating their entries in the control panel, giving them a lot of flexibility.

At the template level, you stay fully in control of the generated HTML code.

We are using a special [`{% switch %}` tag](https://craftcms.com/docs/4.x/dev/tags.html#switch) available for Craft.

```twig
{# Modular Body #}
{% for mxBlock in entry.modularBody %}

  {% switch mxBlock.type %}

    {% case "textBlock" %}

      {{ mxBlock.mxTextTxt }}

    {% case "quoteBlock" %}

      <blockquote>
        <p>{{ mxBlock.mxQuoteTxt }}</p>
        <footer><cite>{{ mxBlock.mxQuoteAuthor }}</cite></footer>
      </blockquote>

    {% case "imageBlock" %}

      {% set image = mxBlock.imageFile.one() %}
      <figure class="figure{% if mxBlock.mxImageFullwidth %} figure--full{% endif %}">
        <img src="{{ image.getUrl(smallThumb) }}" alt="{{ image.title }}" />
        <figcaption class="figure__info">
          <p class="figure__caption">{{ mxBlock.mxImageCaption }}</p>
          <p class="figure__copyright">{{ mxBlock.mxImageCopyright }}</p>
        </figcaption>
      </figure>

  {% endswitch %}

{% endfor %}
```

I personally like to simplify my templates a little bit and put all my Matrix Blocks views in dedicated self-contained files.

By self-contained, I mean that if image transforms or other variables are needed, they will all go inside those files.

That way I can also easily reuse those Matrix blocks templates if needs be.

```twig
{# Modular Body #}
{% for mxBlock in entry.modularBody %}

  {% switch mxBlock.type %}

    {% case "textModule" %}
      {% include '_matrixblocks/text.html' with {
        textBlock: mxBlock
      } only %}

    {% case "quoteModule" %}
      {% include '_matrixblocks/quote.html' with {
        quoteBlock: mxBlock
      } only %}

    {% case "imageModule" %}
      {% include '_matrixblocks/image.html' with {
        imageBlock: mxBlock
      } only %}

    {% endswitch %}

{% endfor %}
```

## Further explorations

Here are some techniques and concepts you might want to look at to explore Craft a bit further and get more out of it.

### Search

Craft has a [very powerful built-in search system](https://craftcms.com/docs/4.x/searching.html) based on a `search` parameter that can be used with `craft.entries()`, `craft.users()`, `craft.assets()` and `craft.tags()`.

For performance reasons, Craft uses indexes for its search functionalities and those indexes can be updated or rebuilt directly from the control panel.

It is also easy to build [dynamic search forms](https://craftcms.com/knowledge-base/search-form) for your project. All you need to do is to use `craft.app.request.getParam('parameter')` to be able to use the GET/POST parameters passed by your form in the context of your results template.

### Creating complex queries

As we have seen earlier, Craft tags like `craft.entries()` can be passed objects as parameters. Twig, on the other hand, allows you to easily create and manipulate objects using the `merge` and `slice` filters as well as Craft's own `without` and `intersect` filters.

Combining those abilities allows you to [create advanced queries](https://www.webstoemp.com/blog/modular-element-queries-craft-cms/) and relatively complex functionalities with ease.

Here is a small example. Let's say you allowed your users to choose what 3 blogposts to feature on the homepage. You have provided an entry field and you have set the limit to three because your design only has three slots for blogposts on the homepage. What you want is to always fill those three spots with blogposts. You want to start with however many posts the user specified using the entry field and you want to fill the remaining slots with the most recent blogposts. No duplicates allowed. Here is how you would accomplish that.

```twig
{#
 # 1. Create array containing Ids of chosen blogposts
 # 2. If not up to 3 items, get the Ids of the 3 most recent blogposts and remove chosen blogposts Ids from that list
 # 3. Get your final elements using the list of Ids
##}

{% set blogpostsIds = entry.homeProjects.ids() %}

{% if blogpostsIds | length < 3 %}
    {% set recentBlogpostsIds = craft.entries().section('blogposts').limit(3).ids() | without(blogpostsIds) %}
    {% set blogpostsIds = blogpostsIds | merge(recentBlogpostsIds) | slice(0,3) %}
{% endif %}

{% set blogposts = craft.entries()
  .section('blogposts')
  .id(blogpostsIds)
  .fixedOrder(true)
  .all() %}

{# display blogposts #}
{% for item in blogposts %}
  {% if loop.first %}<ul>{% endif %}

    <li>
      <h3><a href="{{ item.url }}">{{ item.title }}</a></h3>
      <p>{{ item.blogpostSummary }}</p>
    </li>

  {% if loop.last %}</ul>{% endif %}
{% endfor %}
```

### Navigation interfaces using structures

If you try to have a (single) entry for each page on your site, Craft makes it very easy to have an updatable, user maintainable primary navigation. I generally use a `structure` section for that. Here is a quick example with a simple one level navigation. We just create a structure called `mainnav` and give it two fields: `mainnavLabel` (textfield) and `mainnavLink` (entries, restricted to one and to singles only). We can then use a simple `for` loop to create our navigation.

```twig
{% set nav = craft.entries()
  .section('mainnav')
  .all() %}

{% for item in nav %}
  {% if loop.first %}<ul clas="c-mainnav">{% endif %}

    {% set currentClass = "" %}
    {% set currentAria = "" %}
    {% set navSection = item.mainnavLink.one() %}
    {% if (entry is defined and navSection.uri == entry.uri) %}
      {% set currentClass = "c-mainnav__link--current" %}
      {% set currentAria = 'aria-current="page"'|raw %}
    {% endif %}

    <li class="c-mainnav__item">
      <a class="c-mainnav__link  {{ navCurrentClass }}" href="{{ navSection.url }}" {{ currentAria }}>{{ item.mainnavLabel }}</a>
    </li>

  {% if loop.last %}</ul>{% endif %}
{% endfor %}
```

That's obviously a very simple use case, but this methodology can be used for a lot of navigation interfaces. In my experience, it starts to break down if you have more than a couple of levels.

### Multilingual websites

Multilingual websites are complex beasts, but Craft makes it relatively easy to tackle them since localisation is built into the core. The basics are simple enough and are detailed in the "[Localization section](https://craftcms.com/docs/4.x/sites.html) of the Craft documentation.

I live and work in Belgium, a country with 3 official languages so Craft's out of the box localisation features have been a real breath of fresh air. It also means I have worked on my fair share of multilingual websites and written a [small blogpost detailing the various helpers and macros I have developed](https://webstoemp.com/blog/craft-multilingual-websites-tips/) over time to make those projects easier to tackle.

Here is how to create a simple but effective language switcher.

```twig
{# get all sites #}
{% set allSites = craft.app.sites.allSites() %}

<nav>
  {# loop through all locales #}
  {% for site in allSites %}
    {% if loop.first %}<ul>{% endif %}

      {#- is looped locale the current one -#}
      {% set activeClass = "" %}
      {% set activeAria = "" %}
      {% if site.language == currentSite.language %}
        {% set activeClass = "is-active" %}
        {% set activeAria = 'aria-current="page"'|raw %}
      {% endif %}

      {#- get entry in other locales or fallback to homepage in that locale -#}
      {% set localisedEntry = craft.entries().id(entry.id).site(site).status('live').one() ?? NULL %}
      {% set linkUrl = localisedEntry ? localisedEntry.getUrl() : site.baseUrl %}

      <li>
        <a class="{{ activeClass }}" {{ activeAria }} href="{{ linkUrl }}">{{ site.language|upper }}</a>
      </li>

    {% if loop.last %}</ul>{% endif %}
  {% endfor %}
</nav>
```

### Query optimisation with with eager-loading

A common problem with databases is known as "the n+1 problem". In a nutshell, this happens when you need to traverse a collection of related objects: for each object in the collection, `n + 1` queries are generated since each object in the collection can have `n` related objects. A simple example in Craft is loading a series of entries, and each entry has a related asset. When it fetches those entries, Craft creates `n` additional queries (one per entry) to check if a related asset exists or not. That's the default behavior, which is called "lazy loading".

"[Eager-loading](https://craftcms.com/docs/4.x/dev/eager-loading-elements.html)" is a way to tell Craft, when you are making that main query for the entries, that each entry has a related asset that it should load too. Craft is then performing a more complex MySQL query under the hood loading all entries and related assets using as few queries as possible. You do that by using the `with` parameter in your `craft.entries()` tag.

**Lazy-loading (assets) with `all()`:**

```twig
{% set items = craft.entries()
  .section('blogposts')
  .all() %}

{% for item in items %}
  <article>
    {% set img = item.image.one() %}
    {% if img %}
      <img src="{{ img.getUrl({ width: 600, height: 450 }) }}" alt="{{ img.alt }}">
    {% endif %}
    <h2><a href="{{ item.url }}">{{ item.title }}</a></h2>
  </article>
{% endfor %}
```

**Eager-loading (assets) with `all()`:**

```twig
{% set items = craft.entries()
  .section('blogposts')
  .with(['blogpostImage'])
  .all() %}

{% for item in items %}
  <article>
    {% set img = item.image[0] %}
    {% if img %}
      <img src="{{ img.getUrl({ width: 600, height: 450 }) }}" alt="{{ img.alt }}">
    {% endif %}
    <h2><a href="{{ item.url }}">{{ item.title }}</a></h2>
  </article>
{% endfor %}
```

As we said earlier, using `.collect()` instead of `.all()` or `.one()` returns a Laravel collection, and allows us to keep a consistent syntax, whether we are eager-loading or not.

**Lazy-loading (assets) with `collect()`:**

```twig
{% set items = craft.entries()
  .section('blogposts')
  .collect() %}

{% for item in items %}
  <article>
    {% set img = item.image.collect().first() %}
    {% if img %}
      <img src="{{ img.getUrl({ width: 600, height: 450 }) }}" alt="{{ img.alt }}">
    {% endif %}
    <h2><a href="{{ item.url }}">{{ item.title }}</a></h2>
  </article>
{% endfor %}
```

**Eager-loading (assets) with `collect()`:**

```twig
{% set items = craft.entries()
  .section('blogposts')
  .with(['blogpostImage'])
  .collect() %}

{% for item in items %}
  <article>
    {% set img = item.image.collect().first() %}
    {% if img %}
      <img src="{{ img.getUrl({ width: 600, height: 450 }) }}" alt="{{ img.alt }}">
    {% endif %}
    <h2><a href="{{ item.url }}">{{ item.title }}</a></h2>
  </article>
{% endfor %}
```

Eager-loading elements can get more complex. You can eager load related elements like assets, entries, categories, tags or users, but you can also eager load assets and transforms, matrix blocks. You can eager load nested related elements too, which cost me a few gray hairs already. [Straight Up Craft has an excellent guide on eager-loading](https://straightupcraft.com/articles/examples-of-eager-loading-elements-in-twig-and-php) if you want to dig deeper into it.

Here is a more complex example to use if each entry has a matrix field containing an asset field for which a `thumbnail` transform is defined in the template and applied:

```twig
{% set thumbnail = {
  mode: 'crop',
  position: 'center-center'
  width: 800,
  height: 450,
  quality: 75
} %}

{% set items = craft.entries()
  .section('blogposts')
  .with([
    ['matrixFieldHandle.blockTypeHandle:assetFieldHandle', {
      withTransforms: ['thumbnail']
    }]
  ])
  .collect() %}

{% for item in items %}
  {# display item #}
{% endfor %}
```

### The `{% cache %}` tag

[Craft's `{% cache %}` tag](https://craftcms.com/docs/4.x/dev/tags.html#cache) can be used to speed up the performance of certain parts of your templates. Cached parts of a template will run needed database queries the first time a user hits the template and store the resulting html in the database. The next time a user hits that template, Craft is only going to fetch the stored HTML instead of running all those queries again.

Craft automatically clears caches when elements within `{% cache %}` and `{% endcache %}` tags are deleted or updated. You can also specify a cache duration in your templates. The default duration if you do not define one using the tag parameters is the one specified by the [`cacheDuration`](https://craftcms.com/docs/4.x/config/config-settings.html#cacheduration) config setting. The default is 24 hours. You can set all caches to never expire unless elements they contain are created, updated or deleted by setting your `cacheDuration` to `false`. That behavior will be overridden for any cache tag with a `for` parameter set to any duration.

```twig
{% cache %}
  {# some code to cache for the duration specified by
  the 'cacheDuration' configuration option #}
{% endcache %}

{% cache for 1 month %}
  {# some code to cache for one month #}
{% endcache %}

{% cache for 3 days %}
  {# some code to cache for three days #}
{% endcache %}

{% cache for 7 hours %}
  {# some code to cache for seven hours #}
{% endcache %}
```

Caching should be used on templates that already have been optimized, for example by using eager-loading. Caching templates or arts of templates that are performing poorly is not a viable long term solution.

That can result in big performance gains. Prime targets for the `{% cache %}` tag are:

- Long lists of entries
- Matrix fields where Matrix blocks have relational fields (entries, assets, users, categories, tags)
- Data pulled from a different site (Twitter, etc.)

You can test the efficiency of your caching strategies by making sure `devMode` is on, clearing out caches in the control panel (settings, tools) and refresh your page while looking at the Console in your browser dev tools. Look at the Profiling Summary Report, it will give you the total number of queries ran and the time it took for Craft to render the page.

The first time you refresh your page, all queries are going to run and the resulting HTML will be written to the database (in the `craft_templatecaches` table specifically). The second time you refresh the page, Craft will only pull out the cached HTML and you should see a sharp decrease in the number of queries and time to render the page.

### Front end entry forms and Guest Entries

Out of the box, Craft does allow you to create [entry forms](https://craftcms.com/knowledge-base/entry-form) on the front-end of your website. These forms can only be used by registered users. You can also allow anonymous users to post entries from your front-end by using a the [Guest Entries](https://github.com/craftcms/guest-entries) first party plugin. That plugin allows you to select which sections you want to authorize guest entries for and what the default author will be for those entries.

The syntax to use for those front-end forms is pretty simple. For the most part, the magic lies in those hidden fields.

Two important things to note:

- If there is a validation error on the entry, the URL is reloaded and an `entry` variable is made available. The `entryModel` describes the submitted entry.
- You can fetch the posted values from that `entry` variable, as well as any validation errors via `entry.getErrors()`, `entry.getFirstError()`, etc.

### Using Craft as a headless CMS

You can also use Craft as a headless CMS, which is simply a CMS delivering content via an API. In that scenario, your CMS does not care how the content is displayed and does not deal with the views or templates, but only with creating, updating, deleting, modifying and organizing the content.

Craft also offers a [GraphQL content API](https://craftcms.com/docs/4.x/graphql.html) that you can easily query from any static site generator or SPA framework. The [official Craft tutorial](https://craftcms.com/docs/getting-started-tutorial/build/graphql.html) and the [documentation](https://craftcms.com/docs/4.x/graphql.html) both have pretty hands on guides about using Craft as a headless CMS with GraphQL.

### Importing data (import plugins)

Craft is a relatively young CMS and most projects these days are relaunches of existing websites rather than creating a website from scratch. Knowing these two facts, the need to import existing data into a new Craft install needs to be addressed in most cases.

Luckily for us, Craft has a great first party import plugin called [Feed Me](https://plugins.craftcms.com/feed-me), which lets you import XML, RSS, ATOM, CSV or JSON feeds.

I generally go about it by creating RSS or JSON feeds in the old install, which most CMS will let you do, and use Feed Me to import nodes as entries in Craft. Some manual work is usually needed to tidy things up but the bulk of the work can often be automated.

## Exercises

### Together

Based on thr static templates I will give you, we are going to build a simple agency site together:

1. Homepage

- single section
- displays the last 3 case studies (channel section)

2. Case studies (archive)

- paginated list with 12 case studies on each page
- case studies can be filtered using categories

2. Case study (detail page)

- Each case study uses 3 Matrix blocks (video, image, text) to allow users to create modular content

4. About page

- single section
- lists team members (channel section)
- lists values (structure section)

### On your own

1. Create the team section, custom fields and display those entries on the about page (ordered by surname).
2. On top of Youtube videos, the case studies Matrix should allow user to to display videos from Vimeo.
3. Add a responsive graphical banner to the about page

## Resources

- [Official documentation](https://docs.craftcms.com/) for Craft.
- [Official Tutorial](https://craftcms.com/docs/getting-started-tutorial/) to get started
- [Twig documentation](http://twig.sensiolabs.org/doc/templates.html) for template designers.
- [Craft stackexchange site](http://craftcms.stackexchange.com/): ask questions, get answers. The whole Pixel&Tonic team is on it.
- [Discord Server](https://craftcms.com/discord): ask questions, help others, chat, etc.
