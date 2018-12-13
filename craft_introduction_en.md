# An introduction to Craft, a CMS by Pixel&Tonic

## Introduction

### Technologies and ethos

[Craft](http://buildwithcraft.com/) is a young and very capable CMS created by [Pixel & Tonic](http://pixelandtonic.com/). Craft is developed using open source technologies like PHP and MySQL and is based on a PHP framework with a great track record: [Yii](http://www.yiiframework.com/).

Craft focuses on the essentials: defining and managing your content in the most modular and flexible way possible. For example, localisation is built in from the get go but if you need comments, a forum or e-commerce, you will have to turn to plugins.

### A modular pricing model

This flexibility is present into the [pricing model](https://craftcms.com/pricing) of the product itself.

- The "Solo" version of Craft is free. You can enjoy all functionalities of Craft except for its user management and permissions. The only account available is an admin account.
- The "Pro" version costs $299 and offers you full user management as well as the possibility brand the system.

#### Licenses

Craft licenses are perpetual, meaning you can use the version you purchased indefinitely. You also get one year of Developer support and updates bundled with your purchase.

If you want updates and support after that, you will have to pay $59 for one year of upgrades and support. You can pay that license fee whenever you need it.

#### Trial versions

[You can test Craft Pro and all its functionalities](https://craftcms.com/guides/try-craft-pro-plugins-before-buying) for an indefinite amount of time provided that you are on a local or stanging domain that can be identified as such. Personally, I use `myprojectname.craft.test` for all my local projects.

### A fully integrated e-commerce platform

Pixel & Tonic released a first party e-commerce module for Craft dubbed [Craft Commerce](https://craftcms.com/commerce). A one-time license of Craft Commerce for one site is $999. For that price, you get [a ton of features](https://craftcms.com/features) and everything you need to create and manage an online store.

A "Lite" version of Commerce is also available with much simpler e-commerce capabilities for a price of $199 per project.

Covering Craft Commerce is not in the scope of this introduction to Craft but just know Craft Commerce shares a lot of characteristics with Craft core as far as data structure creation and templating are concerned so you won't feel lost. If you need tight e-commerce integration with your CMS, Craft Commerce is definitely an option to consider.

### Strengths

In my opinion, the main strengths of Craft are:

1. An unparalleled flexibility in the definition of your item types and of their data structure. The 16 field types available by default allow for an extremely modular approach of your content.
2. Speaking of modular fields, Matrix certainly is Craft's main asset in terms of creating a flexible data structure while maintaining total control over the generated front-end code.
3. Using [Twig as its templating language](http://twig.sensiolabs.org/doc/templates.html). Learning Twig certainly has a learning curve but the benefits in terms of power, performance, modularity and flexibility for your templates are amazing.
4. An integrated and comprehensive solution for multilingual sites with great localisation features.
5. An impressive amount of features available out of the box, designed to make your life and the life of your users easier: live preview, responsive control panel, one click updates, multiple environments configurations, etc.
6. Freedom to build your own URL structure: there is almost no relation between your URL structure and your folder and files structure, thanks to a very flexible routing mechanism.
7. A fantastic development and support team.

## 1. Installation and configuration

### Setting things up

After checking your server has everything it needs to run Craft, all you have to do is [follow a simple procedure](https://docs.craftcms.com/v3/installation.html) to install Craft and check your permissions. This should go smoothly.

### Version control

Any developer worth its salt is version controlling its work using Git these days. Craft has its own .gitignore file (https://github.com/craftcms/craft/blob/master/storage/.gitignore). You only have to take care of your own files. This is the .gitignore file I generally use as a starting point:

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
web/dist/

# node modules
# ----------------------
node_modules/
```

### Multiple environments configurations

Craft offers a native way to deal with multiple environments configurations through the use of nested arrays and [environment-specific variables ](https://docs.craftcms.com/v3/config/environments.html) in your `config/general.php` and `config/db.php` configuration files.

Because your developers likely use different files and folders architectures locally and because sensitives informations like database credentials should ideally not be committed to a repository, Craft allows you to use a `.env` file at the root of your project so every developer can use her/his own settings. You can then use values specified in that `.env` files in `craft/config/general.php` and `craft/config/db.php`.

Here is an  exemple of what it looks like:

**Example**: `.env`

```
# The environment Craft is currently running in ('dev', 'staging', 'production', etc.)
ENVIRONMENT="dev"

# The secure key Craft will use for hashing and encrypting data
SECURITY_KEY="superlonghashkey"

# The database driver that will be used ('mysql' or 'pgsql')
DB_DRIVER="mysql"

# The database server name or IP address (usually this is 'localhost' or '127.0.0.1')
DB_SERVER="localhost"

# The database username to connect with
DB_USER="root"

# The database password to connect with
DB_PASSWORD="mypassword"

# The name of the database to select
DB_DATABASE="mycraftdb"

# The port to connect to the database with. Will default to 5432 for PostgreSQL and 3306 for MySQL.
DB_PORT="3306"

# Base URL (no trailing slash)
BASE_URL="http://myproject.craft.test"

# Base PATH (no trailing slash)
BASE_PATH="/data/weblocal/myproject/web"
```

In `craft/config/general.php` and `craft/config/db.php`, start by defining a `*` array. Values specified in there will be applied to all your environments. The `*` array is mandatory, even if it contains nothing, as Craft looks for it to enable multi-environment config support. Other array keys will reference environments defined in your `.env` file. Craft will check those keys against the CRAFT_ENVIRONMENT PHP constant and default to "production" if nothing is specified. You can pretty much override any [configuration settings](https://docs.craftcms.com/v3/config/config-settings.html) that way.

You can also use `craft/config/general.php` to create Yii aliases. You can then use those aliases in the Control Panel, for example when defining file system paths and url for your asset volumes to make them environment specific. You can also reference those aliases in your templates using the [Craft `alias` function](https://docs.craftcms.com/v3/dev/functions.html#alias-string).

**Example**: `config/general.php`

```
<?php
/**
 * General Configuration
 *
 * All of your system's general configuration settings go in here. You can see a
 * list of the available settings in vendor/craftcms/cms/src/config/GeneralConfig.php.
 *
 * @see craft\config\GeneralConfig
 */

return [
    // Global settings
    '*' => [
      // Default Week Start Day (0 = Sunday, 1 = Monday...)
      'defaultWeekStartDay' => 1,

      // Enable CSRF Protection (recommended)
      'enableCsrfProtection' => true,

      // Whether generated URLs should omit 'index.php'
      'omitScriptNameInUrls' => true,

      // Control Panel trigger word
      'cpTrigger' => 'admin',

      // The secure key Craft will use for hashing and encrypting data
      'securityKey' => getenv('SECURITY_KEY'),
    ],

    // Dev environment (defined in .env ENVIRONMENT)
    'dev' => [
      'devMode' => true,
      'aliases' => [
        '@environment' => getenv('ENVIRONMENT'),
        '@baseUrl' => getenv('BASE_URL'),
        '@assetsBaseUrl' => getenv('BASE_URL').'/uploads',
        '@assetsBasePath' => getenv('BASE_PATH').'/uploads',
      ]
    ],

    // Production environment settings (Craft default or server environment value)
    'production' => [
      'aliases' => [
        '@environment' => 'production',
        '@baseUrl' => 'https://myproject.com',
        '@assetsBaseUrl' => 'https://myproject.com/uploads',
        '@assetsBasePath' => '/myproject.com/web/uploads',
      ],
    ],
];
```

**Example**: `config/db.php`. Everything is defined via .env or via server environment values in production.
```
return array(
  'driver' => getenv('DB_DRIVER'),
  'server' => getenv('DB_SERVER'),
  'user' => getenv('DB_USER'),
  'password' => getenv('DB_PASSWORD'),
  'database' => getenv('DB_DATABASE'),
  'schema' => getenv('DB_SCHEMA'),
  'tablePrefix' => getenv('DB_TABLE_PREFIX'),
  'port' => getenv('DB_PORT')
);
```

**Example**: usage in the control panel for assets (local) volumes definition.

```
{@assetsBasePath}/partners_logos/
{@assetsBaseUrl}/partners_logos/
```

**Example**: usage in templates.

```twig
{% if alias('@environment') == 'production' %}
  {# include Google Analytics code #}
{% endif %}
```

Of course, values defined via dotEnv and used in your production configuration must be available to Craft in your production environment. That is usually done directly in your webserver configuration, be it Apache or Nginx. `.env` file are not used in production and hosting providers will offer you a way to configure environment variables at the server level.

### Rich text configurations

Craft allows you to use [CKEditor](https://ckeditor5.github.io/) or [Redactor](http://imperavi.com/redactor/) as WYSIWYG solutions if you want your users to be able to enter rich text. Both are available as free plugins in the plugin store.

I have always used Redactor with pretty simple configurations.

Redactor configurations can be easily created and modified with JSON files stored in the `config/redactor/` folder. The filenames of your JSON files will be the names of configurations available in the Control Panel for the Redactor field.

## 2. Create your data structure

Craft allows you to create very flexible and modular data structures for your projects.

### Sections, Entries and entry types

With Craft, your content will mainly live in entries. Those entries are grouped under entry types to form sections.

The data structure of those entries is created by assigning custom fields to the entry types you defined for each sections. For each of those entry types, you can create a field layout defining which fields will be used by all entries of that type in a given section.

There are [three types of sections](https://docs.craftcms.com/v3/sections-and-entries.html): singles, channels and structures.

#### Single sections

These sections contain only one entry type and only one entry. They are used to create one off pages in your site, like an "about page" or a "homepage".

Using the section configuration screen you can define

- The URL format for the entry.
- The template Craft should load to display that entry.

The field layout screen allows you to assign custom fields to your entry type in order to define the data structure of your entry.

#### Channel sections

Think of those sections as streams of entries that can be filtered, ordered and displayed in various ways using your templates (news, case studies, press releases, pictures, videos, etc.).

Using the section configuration screen you can define

- The URL format for all entries in that section.
- The template Craft should load to display entries belonging to that section.

Channel sections can contain various entry types, each having their own data structure. For example creating a blog allowing you to post various types of content is very easy. All you need is a channel section with various entry types (blogposts, links, videos, pictures, etc.)

For each entry type defined in your section, you can define the data structure of entries by assigning custom fields to a field layout.

These entry types can easily be [used in routing and URL structures](https://craftcms.com/guides/entry-type-urls) as well as in your templates with [`craft.entries` tags and conditionals](https://docs.craftcms.com/v2/templating/entrymodel.html#properties).

#### Structure sections

Structure sections behave a lot like channel sections: they can contain various entry types and the URL structure of their entries can be specified. The main difference is that their entries can be manually ordered in a hierarchical tree. You can  specify the number of levels in that hierarchy.

Using the section configuration screen you can define

- The URL format for entries in that section. Different URL pattern can be specified for top-level entries and for nested entries.
- The template Craft should load to display entries belonging to that section.

For each entry type defined in your section, you can define the data structure of entries by assigning custom fields to a field layout.

### Fields, Field Groups and Field Layouts

Craft comes natively with [numerous field types](https://docs.craftcms.com/v3/fields.html) through which you can define the data structure of your entries.

A field can be applied to several entries, users, assets, tags, categories or globals via a "field layout" allowing you to perform operations on the fields (ordering, make mandatory or not, etc) via a drag and drop interface.

Fields can be grouped into field groups. These groups are there purely for convenience sake to help you manage big projects more easily.

### Globals

Next to sections and entries, [globals](https://docs.craftcms.com/v3/globals.html) can be used to store bits of content or options you want your client to be able to edit easily: tagline, contact data, Google Analytics code, number of entries to display in lists, etc.

Those globals can be grouped together using global sets. Each global set has its own field layout and thus its own data structure.

### Users

Craft has [powerful and granular user management](https://docs.craftcms.com/v3/users.html) allowing you to manage the permissions for all users of the system.

Users can be assigned to various user groups. Permissions can be managed at a user group or at a user level. A user can be assigned to multiple groups.

The data structure for your users can be defined easily. A unique field layout allows you to assign custom fields to all your users, regardless of their groups.

### Assets

[Assets](https://docs.craftcms.com/v3/assets.html) allow you to manage your files with Craft (images, videos, sounds, PDFs, etc). Assets are assigned to Asset Sources corresponding to folders on your server, or on external Cloud servers like Rackspace or Amazon S3 if you are the proud owner of a Craft Pro license.

Craft allows you to apply [transforms](https://docs.craftcms.com/v3/image-transforms.html) (crop, fit, scale, quality, etc.) to images automatically, either in the control panel or dynamically through templates. That feature allows you to generate all the thumbnails you needs from an initial image.

A field layout is available for each Asset Source. Using it in combination with custom fields allow you to create complex data structure for each of your assets types. For example, documents can have a different data structure than photos.

### Tags

[Tags](https://docs.craftcms.com/v3/tags.html) allow you to create *folksonomies* and apply them to your Entries, Users or Assets.

Every tag must be assigned to a group and each tag group has a field layout. You can create complex data structures for each of your tag groups if needed.

### Categories

[Categories](https://docs.craftcms.com/v3/categories.html) allow you to create *taxonomies* and apply them to your Entries, Users or Assets.

Each category must be assigned to a group and each of them has a dedicated field layout.

For each category group, the edit screen allows you to:

- Specify the URL structure for categories and sub categories
- Specify the template that Craft will load to display when a category URL is requested.

### Relations

One of Craft's great strengths is its [relations system](https://docs.craftcms.com/v3/relations.html). You can easily create relations between Entries, Users, Assets and Tags through a series of relational field types:

- **Assets**: allows you to establish a "one to one" or "one to many" relation to Assets.
- **Entries**: allows you to establish a "one to one" or "one to many" relation to Entries.
- **Users**: allows you to establish a "one to one" or "one to many" relation to Users.
- **Tags**: allows you to establish a "one to one" or "one to many" relation to Tags.
- **Categories**: allows you to establish a "one to one" or "one to many" relation to Categories.

For each of those field, you can specify how many items can be linked and from what source(s) they come from.

To display and work with these relations in your templates, Craft is giving you a very powerful tool in the form of the [`relatedTo`](https://docs.craftcms.com/v3/relations.html#the-relatedto-parameter) parameter. You can use that parameter with `craft.entries`, `craft.users`, `craft.assets`, `craft.tags` and `craft.categories`.

### Routing

Another interesting aspect of Craft is its [dynamic routing system](https://docs.craftcms.com/v3/routing.html), which allows you separate your URL structure from your folders and files structure.

At the most basic level, we have already seen that Craft allows you to specify the URL structure of each of your Entries, Users, Assets, Tags and Categories.

If that's not enough to cover all your needs, you can also create dynamic routes independently. For each route you create, you can specify which template must be loaded by Craft. An easy to understand example is [a template giving access to a yearly archive of entries](https://craftcms.com/guides/creating-an-archive-page-for-entries#yearly-archive-pages).

When you create a dynamic route with the structure of `blog/archive/{year}` calling the template `blog/archive`, the URLs `blog/archive/2013` and `blog/archive/2012` will load the same template and make a `{year}` variable accessible to to Twig in that template. You can for example use it with the [`after`](https://docs.craftcms.com/v3/dev/element-queries/entry-queries.html#after) and [`before`](https://docs.craftcms.com/v3/dev/element-queries/entry-queries.html#before) parameters of `craft.entries()`.

If you need more possibilities than those the Control Panel gives you, you can [manage your routes in a more advanced way](https://docs.craftcms.com/v3/routing.html#advanced-routing-with-url-rules) using the `config/routes.php` file. That will give you access to all the power of regular expressions in your URL matching patterns.

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

Create an `allEntries` variable and assign it the results of an [ElementQuery](https://docs.craftcms.com/v3/dev/element-queries/) containing the last 10 entries in the `blog` section, ordered by creation date in descending order.

```twig
{% set allEntries = craft.entries().section('blog').limit(10).order('postDate desc').all() %}
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
{% set allEntries = craft.entries().section('blog').order('postDate desc').all() %}
```

### Filters

[Twig](http://twig.sensiolabs.org/doc/filters/index.html) and [Craft](https://docs.craftcms.com/v3/dev/filters.html) have filters that can be applied to your variables and modify or manipulate them. It means that Craft doesn't need any plugin to perform simple tasks in your templates. Here are some examples of what can be done using filters:

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
{% set allEntries = craft.entries().section('blog').order('postDate desc').all() %}
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

[Twig](http://twig.sensiolabs.org/doc/functions/index.html) and [Craft](https://docs.craftcms.com/v3/dev/functions.html) both have functions allowing you to execute functions on your data.

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

`dump()` is only accessible in Craft when [Dev Mode is enabled](https://craftcms.com/guides/what-dev-mode-does) in your `config/general.php` file. Essential for debugging.

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

{% if allEntries|length %}
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
{% if currentUser and currentUser.male %}
{% if superAdmin or admin %}
```

**Loop**

```twig
{% for entry in allEntries %}
  {% if loop.first %}<ul>{% endif %}
    <li>
      <article>
        <h2>{{ entry.title }}</h2>
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

Twig allows you to control how your code is displayed, including [at the whitespace level](http://twig.sensiolabs.org/doc/templates.html#whitespace-control), which, if you are using `inline-block`, is a godsend.

### Template inheritance, includes and macros: stay DRY

These three concepts are at the heart of Twig as a templating language. They enable you to create efficient templates and avoid redundancy, allowing you to comply with the infamous "Don't Repeat Yourself" (DRY) principle.

#### Template inheritance

Template inheritance is a central concept in Twig, as it is in many other templating languages for the web. We will generally work with a "parent" template defining the skeleton of the page, its different blocks and one or more "children" templates. These children templates will extend the parent template and define the content of each of those blocks.

Variables defined in "children" templates can be accessed in the "parent" template.

Let's see how it works practically with a very simple example:

**Parent template: layouts/_base.html**

```twig
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="utf-8">

  <title>{% if htmlTitle is defined %}{{ htmlTitle }}{% else %}My generic title{% endif %} - Mysite</title>

  <meta name="viewport" content="width=device-width, initial-scale=1">

  <link rel="shortcut icon" href="{{ siteUrl }}/favicon.ico">
  <link rel="stylesheet" media="screen" href="{{ siteUrl }}/assets/css/screen.css">

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
{% set htmlTitle = "Latest News" %}

{% block content %}

  <h1>News</h1>

  {% for entry in craft.entries().section('news').all() %}
    <article>
      <h3><a href="{{ entry.url }}">{{ entry.title }}</a></h3>
      <p>Posted on {{ entry.postDate.format('F d, Y') }}</p>
      {{ entry.body }}
      <p><a href="{{ entry.url }}">Continue reading</a></p>
    </article>
  {% endfor %}

{% endblock %}
```

The `news/index.html` template extends the base layout template. The content defined in the `content` block of the child template replaces / supersedes the content of the `content` block in the parent template.

As a side note, the name of the "parent" template begins with an underscore to tell Craft that template is hidden and cannot be accessed directly via a web browser.

Typically, your layouts, includes and entry templates should all be hidden.

#### Includes

If you have code that is repeated in many templates, it is generally a good idea to use includes `{% include %}`. Since Twig compiles everything down to raw PHP, there is no real performance penalty for using includes.

```twig
{% include '_sidebars/default.html' %}
```

#### Macros

Twig [Macros](http://twig.sensiolabs.org/doc/tags/macro.html) are comparable to mixins in Sass. They are small reusable chunks of code.

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

If the Macro is defined in the same file, the `_self` keyword is used

```twig
{% import _self as formErrors %}
{{ formErrors.errors(entry.allErrors) }}
```

If the macro is defined in an external file, we simply reference the path to the file

```twig
{% import "_macros/dates" as dateHelpers %}
{{ dateHelpers.dateText(entry.postDate) }}
```

## 4. Retrieve, manipulate and display your data

In Craft, you interact with the database using [Element Queries](https://docs.craftcms.com/v3/dev/element-queries/). It sounds complicated but it is in fact quite simple:

1. you create an ElementQuery for the type of data you want to get from the database (entries, users, assets, etc.)
2. you specify the parameters (limit, order, filters, etc.) you want to use.
3. Your execute the ElementQuery by using the following function: `.all()`, `one()`, `exists()`, `.count()` or `.ids()`
4. Craft returns an element or an array of elements objects ([entry element](https://docs.craftcms.com/api/v3/craft-elements-entry.html), [user element](https://docs.craftcms.com/api/v3/craft-elements-user.html), [asset element](https://docs.craftcms.com/api/v3/craft-elements-asset.html), [category element](https://docs.craftcms.com/api/v3/craft-elements-category.html) ou [tag element](https://docs.craftcms.com/api/v3/craft-elements-tag.html)).
5. You can then display those objects or arrays of objects in your template.

`craft.entries()`, `craft.users()`, `craft.assets()`, `craft.categories()` and `craft.tags()` will be your main tools to retrieve and display your data.

We will mainly work with `craft.entries()` in this introduction. Since all tags use the same principles, it will be easy for you to apply what you know to `craft.users()`, `craft.assets()`, `craft.categories()` and `craft.tags()`.

### Entries

[`craft.entries()`](https://docs.craftcms.com/v3/dev/element-queries/entry-queries.html) is going to be your main tool to retrieve and display your entries.

- `craft.entries().all()` allows you to retrieve all entries corresponding to the specified criteria.
- `craft.entries().one()` allows you to retrieve the first entry corresponding to the specified criteria (retunrs `null` if no entry corresponds). [In most cases](https://docs.craftcms.com/v3/changes-in-craft-3.html#last), if you want the last entry, you can use `craft.entries().inReverse().one()`
- `craft.entries().nth(number)` allows you to retrieve the nth entry corresponding to the specified criteria (retunrs `null` if no entry corresponds).
- `craft.entries().exists()` allows you to see if there is at least one entry corresponding to the specified criteria (returns `true` or `false`).
- `craft.entries().count()` allows you to retrieve the total number of entries corresponding to the specified criteria.
- `craft.entries().ids()` allows you to retrieve the ids of all entries corresponding to the specified criteria.

#### A very simple element query

Here is a simple example of an entry element query.

```twig
{% set allEntries = craft.entries().section('news').limit(4).all() %}

{% for entry in allEntries %}

  <h2>{{ entry.title }}</h2>
  {{ entry.summary }}

{% endfor %}
```

You can also build much more complex Element Queries using [Advanced Element Queries](https://docs.craftcms.com/v3/dev/element-queries/#advanced-element-queries).

#### No results

You can easily display alternate content if no entries are found, just by using [an `{% else %}` clause in your `{% for %}` loop](http://twig.sensiolabs.org/doc/tags/for.html#the-else-clause).

```twig
{% set allEntries = craft.entries().section('news').limit(4).all() %}

{% for entry in allEntries %}

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
{% set allEntries = craft.entries().section('news').limit(4).all() %}


{% for entry in allEntries %}

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
{% set allEntries = craft.entries().section('news').limit(4).all() %}


{% for entry in allEntries %}

  {% if loop.first %}<ul>{% endif %}

  <li class="{{ cycle(['odd', 'even'], loop.index0) }}">
    <h2>{{ entry.title }}</h2>
    {{ entry.summary }}
  </li>

  {% if loop.last %}</ul>{% endif %}

{% endfor %}
```

**Example**: using the `is divisible by` test to insert an element every 2 iterations.

```twig
{% set allEntries = craft.entries().section('news').limit(4).all() %}

{% for entry in allEntries %}

  {% if loop.first %}<div class="row">{% endif %}

    <div class="item">
      <h2>{{ entry.title }}</h2>
      {{ entry.summary }}
    </div>

  {% if loop.index is divisible by(2) %}</div><div class="row">{% endif %}

  {% if loop.last %}</div>{% endif %}

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

Craft allows you to [paginate your results](https://docs.craftcms.com/v3/dev/tags/paginate.html) using the `{% paginate %}` tag and to build simple or more complex pagination interfaces using the related variables. One small caveat: the `{% paginate %}` tag needs an ElementQuery as parameter. Just don't call `all()` on it in this case.

```twig
{% paginate craft.entries().section('news').limit(5) as paginate, entries %}

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

#### Category page and "category" variable

The same logic applies with categories. When Craft is loading an URL corresponding to one of the category URL you specified when creating your category groups, the system will automatically populate a `category` variable and make it accessible in our template.

```twig
{#
 # This template gets loaded whenever a Category URL is
 # requested. That’s because a Category group Template setting is
 # set to “news/index”, the path to this template.
 #}

{# layout used #}
{% extends "layouts/_base.html" %}

{% block content %}

 {#
  # - this page is also an entry (single)
  # - when a category route is called, an 'entry' variable is not created
  # - we create the entry variable by hand if not defined
  #}

  {% if entry is not defined %}
    {% set entry = craft.entries().id(7).one() %}
  {% endif %}

 {#
  #  - craft automatically creates a 'category' variable if it detects you are on a category template
  #  - we are just checking whether that category variable exists or not
  #  - depending on its existence, we set our list of entries
  #}

  {% set allCategories = craft.categories().group('newsTopics').all() %}

  {% if category is defined %}
    {% set currentCategory = category.slug %}
    {% set allNews = craft.entries().section('news').relatedTo(category).limit(10) %}
  {% else %}
    {% set currentCategory = 'all' %}
    {% set allNews = craft.entries().section('news').limit(10) %}
  {% endif %}

  {# display page title using entry variable #}
  {{ entry.pageTitle }}

  {# display entries list #}
  {% paginate allNews as paginate, entries %}

  {% for entry in entries %}
    {% if loop.first %}<ul>{% endif %}
      <article>
        <p class="meta-info"><time datetime="{{ entry.postDate|date("Y-m-d") }}">{{ entry.postDate|date("F j, Y") }}</time></p>
        <h2><a href="{{ entry.url }}">{{ entry.title }}</a></h2>
        <p>{{ entry.summary }}</p>
      </article>
    {% if loop.last %}</ul>{% endif %}
  {% else %}
    <p>No news found</p>
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

  {# display categories list #}
  {% for category in allCategories %}
    {% if loop.first %}
      <ul>
        <li><a href="{{ siteUrl }}news/"{% if currentCategory == "all" %} class="current"{% endif %}>All Categories</a></li>
    {% endif %}
        <li><a href="{{ category.url }}"{% if currentCategory == category.slug %} class="current"{% endif %}>{{ category.title }}</a></li>

    {% if loop.last %}</ul>{% endif %}
  {% endfor %}

{% endblock %}
```

### Globals

Globals are used to store content that will be made globally accessible to all your templates. They are typically used to create configuration options or variables that need to be editable by administrators but that do not belong in sections and entries.

Globals can be accessed easily via their global set handle followed by their global handle. For example, a global called `tagline` in a `companyInfo` global set would be accessed this way:

`{{ companyInfo.tagline }}`

### Tags

You can access and display tags using `craft.tags()` and its [related parameters](https://docs.craftcms.com/v3/dev/element-queries/tag-queries.html#parameters). It works just like `craft.entries()` but returns a [`Tag`](https://docs.craftcms.com/api/v3/craft-elements-tag.html) element or an array of those.

Articles in the help section are showing you how to list [all the tags used by the entries in a given section](https://craftcms.com/guides/displaying-tags-that-are-in-use) or how use a dynamic route to create an [archive page listing all the entries related to a tag](https://craftcms.com/guides/assigning-urls-to-tags).

### Users

`craft.users()` allows you to access and display the users of your website. The tag functions like `craft.entries()` but returns a single [`User`](https://docs.craftcms.com/api/v3/craft-elements-user.html) element or an array of those. The `craft.users` tag also has [a series of parameters](https://docs.craftcms.com/v3/dev/element-queries/user-queries.html#parameters), some of which are tied to users-specific functionalities or behaviours.

### Assets and transforms

`craft.assets()` allows you to access your assets. This tag also has a series of [parameters](https://docs.craftcms.com/v3/dev/element-queries/asset-queries.html#parameters), some of which are tied to assets-specific functionalities or behaviour. `craft.assets` works like `craft.entries()` except that it returns a single [`Asset`](https://docs.craftcms.com/api/v3/craft-elements-asset.html) element or an array of those.

If your assets are images, Craft allows you to create transforms tied to all your asset groups. Transforms will generate thumbnails for all your assets. Transforms can be specified in the control panel and generated when assets are uploaded (Settings > Assets > Image Transforms) or they can be specified in your template and generated dynamically when assets are requested for the first time.

When you define a transform in the Control Panel and you name it `thumbnail`, you can access them in your templates in the following way:

```twig
{% set heroImage = entry.myAssetField.one() %}
{% if heroImage %}
  <img src="{{ heroImage.getUrl('thumbnail') }} width="{{ asset.getWidth('thumbnail') }}" height="{{ asset.getHeight('thumbnail') }}" alt="{{ heroImage.title }}">
{% endif %}
```

You can also specify your transforms directly in your templates

```twig
{% set transform = {
  width: 100,
  height: 100,
  mode: 'crop',
  position: 'top-center'
} %}

{% set heroImage = entry.myAssetField.one() %}
{% if heroImage %}
  <img src="{{ heroImage.getUrl(transform) }} width="{{ asset.getWidth(transform) }}" height="{{ asset.getHeight(transform) }}" alt="{{ heroImage.title }}">
{% endif %}
```

### Matrix

[Matrix](https://craftcms.com/features/all#matrix) certainly is one of the most interesting field type available with Craft. Essentially, it allows you to define a data structure for several types of content blocks (using Craft's other field types) and then to combine and arrange these blocks of content.

For example, you could create a `modularBody` Matrix field with the following configuration:

- `textModule` block type
  - `textContent`rich text field (redactor)
- `quoteModule` block type
  - `quoteText` Textfied (256)
  - `quoteAuthor` Textfied (128)
- `imageModule`block type
  - `imageFile` Asset file (limited to 1)
  - `imageCaption` Textfied (128)
  - `imageCopyright` Textfied (128)
  - `imageFullwidth` Lightswitch field

Such a Matrix field would allow your users to combine and arrange those text, image and quote blocks when creating their entries in the control panel, giving them a lot of flexibility.

At the template level, you stay fully in control of the generated HTML code. We are using a special [`{% switch %}` tag](https://docs.craftcms.com/v3/dev/tags/switch.html) available for Craft.

```twig
{# Modular Body #}
{% for module in entry.modularBody %}

  {% switch module.type %}

    {% case "textModule" %}

      {{ module.textContent }}

    {% case "quoteModule" %}

      <blockquote>
        {{ module.quoteText }}
        <footer><cite>{{ module.quoteAuthor }}</cite></footer>
      </blockquote>

    {% case "imageModule" %}

      {% set image = module.imageFile.one() %}
      <figure class="figure{% if module.imageFullwidth %} figure--full{% endif %}">
        <img src="{{ image.getUrl(smallThumb) }}" alt="{{ image.title }}" />
        <figcaption class="figure__info">
          <p class="figure__caption">{{ module.imageCaption }}</p>
          <p class="figure__copyright">{{ module.imageCopyright }}</p>
        </figcaption>
      </figure>

    {% endswitch %}

{% endfor %}
```

I personally like to simplify my templates a little bit and put all my Matrix Blocks views in dedicated self-contained files. By self-contained, I mean that if image transforms or oher variables are needed, they will all go inside those files. That way I can also easily reuse them elsewhere if needs be.

```twig
{# Modular Body #}
{% for module in entry.modularBody %}

  {% switch module.type %}

    {% case "textModule" %}
      {% include '_matrixblocks/textmodule.html' %}

    {% case "quoteModule" %}
      {% include '_matrixblocks/quotemodule.html' %}

    {% case "imageModule" %}
      {% include '_matrixblocks/imagemodule.html' %}

    {% endswitch %}

{% endfor %}
```

## Further explorations

Here are some techniques and concepts you might want to look at to explore Craft a bit further and get more out of it.

### Search

Craft has a [very powerful built-in search system](https://docs.craftcms.com/v3/searching.html) based on a `search` parameter that can be used with `craft.entries()`, `craft.users()`, `craft.assets()` and `craft.tags()`.

For performance reasons, Craft uses indexes for its search functionalities and those indexes can be updated or rebuilt directly from the control panel.

It is also easy to build [dynamic search forms](https://docs.craftcms.com/v3/dev/examples/search-form.html) for your project. All you need to do is to use `craft.app.request.getParam('parameter')` to be able to use the GET/POST parameters passed by your form in the context of your results template.

### Creating complex queries

As we have seen earlier, Craft tags like `craft.entries()` can be passed objects as parameters. Twig, on the other hand, allows you to easily create and manipulate objects using the `merge` and `slice` filters as well as Craft's own `without` and `intersect` filters.

Combining those abilities allows you to [create advanced queries](https://webstoemp.com/blog/manipulating-craft-elementcriteriamodel-with-twig/) and relatively complex functionalities with ease.

Here is a small example. Let's say you allowed your users to choose what 3 blogposts to feature on the homepage. You have provided an entry field and you have set the limit to three because your design only has three slots for blogposts on the homepage. What you want is to always fill those three spots with blogposts. You want to start with however many posts the user specified using the entry field and you want to fill the remaining slots with the most recent blogposts. No duplicates allowed. Here is how you would accomplish that.

```twig
{#
 # 1. Create array containing Ids of chosen blogpsts
 # 2. If not up to 3 items, get the Ids of the 3 most recent blogposts and remove chosen blogposts Ids from that list
 # 3. Get your final ElementCriteria model using your list of Ids
##}

{% set blogpostsIds = entry.homeProjects.ids() %}

{% if blogpostsIds | length < 3 %}
    {% set recentBlogpostsIds = craft.entries().section('blogposts').limit(3).ids() | without(blogpostsIds) %}
    {% set blogpostsIds = blogpostsIds | merge(recentBlogpostsIds) | slice(0,3) %}
{% endif %}

{% set blogposts = craft.entries().section('blogposts').id(blogpostsIds).fixedOrder(true).all() %}

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
{% set nav = craft.entries().section('mainnav').all() %}
{% for item in nav %}
  {% if loop.first %}<ul clas="c-mainnav">{% endif %}

    {% set navSection = item.mainnavLink.one() %}
    {% set navCurrentClass = (entry is defined and navSection.uri == entry.uri) ? "c-mainnav__link--current" : "" %}

    {% if single %}
      <li class="c-mainnav__item">
        <a class="c-mainnav__link  {{ navCurrentClass }}" href="{{ navSection.url }}">{{ item.mainnavLabel }}</a>
      </li>
    {% endif %}

  {% if loop.last %}</ul>{% endif %}
{% endfor %}
```

That's obviously a very simple use case, but this methodology can be used for a lot of navigation interfaces. In my experience, it starts to break down if you have more than a couple of levels.

### Multilingual websites

Multilingual websites are complex beasts, but Craft makes it relatively easy to tackle them since localisation is built into the core. The basics are simple enough and are detailed in the "[Localization section](https://docs.craftcms.com/v3/localization.html) of the Craft documentation.

I live and work in Belgium, a country with 3 official languages so Craft's out of the box localisation features have been a real breath of fresh air. It also means I have worked on my fair share of multilingual websites and written a [small blogpost detailing the various helpers and macros I have developed](https://webstoemp.com/blog/craft-multilingual-websites-tips/) over time to make those projects easier to tackle.

Here is how to create a simple but effective language switcher.

```twig
{# get all sites in the currentSite group #}
{% set allSites = currentSite.group.sites %}

{# loop through all sites #}
{% for site in allSites %}
  {% if loop.first %}<ul class="c-lggnav">{% endif %}
    {# get short language code using macro#}
    {% set lgg = helpers.languageShort(site.language)|upper %}

    {# if site.language == currentSite.language set active class #}
    {% set activeClass = (site.language == currentSite.language) ? 'is-active' : '' %}

    {# check if there is a localised entry #}
    {% set localisedEntry = craft.entries.id(entry.id).site(site).one() %}

    {# set URL value to localisedEntry.url if there is one, otherwise, use homepage #}
    {% set url = (localisedEntry is defined) ? localisedEntry.url : site.siteUrl %}

    <li class="c-lggnav__item"><a href="{{ url }}" class="c-lggnav__link  {{ activeClass }}">{{ lgg }}</a></li>

  {% if loop.last %}</ul>{% endif %}
{% endfor %}
```

### Query optimisation with with eager-loading

A common problem with databases is known as "the n+1 problem". In a nutshell, this happens when you need to traverse a collection of related objects: for each object in the collection, `1 + n` queries are generated since each object in the collection can have `n` related objects. A simple example in Craft is loading a series of entries, and each entry has a related asset. When it fetches those entries, Craft creates `n` additional queries (one per entry) to check if a related asset exists or not. That's the default behaviour, which is called "lazy loading".

"[Eager-loading](https://docs.craftcms.com/v3/dev/eager-loading-elements.html)" is just a way to tell Craft, when you are making that main query for the entries, that each entry has a related asset that it should load too. Craft is then performing a more complex MySQL query under the hood loading all entries and related assets using as few queries as possible. You do that by using the `with` parameter in your `craft.entries()` tag.

**Without asset eager loading:**

```twig
{% set items = craft.entries()
  .section('blogposts')
  .all() %}

{% for item in items %}
  {# display item #}
{% endfor %}
```

**With asset eager loading:**

```twig
{% set items = craft.entries()
  .section('blogposts')
  .with(['blogpostImage'])
  .all() %}

{% for item in items %}
  {# display item #}
{% endfor %}
```

Eager loading elements can get more complex. You can eager load related elements like assets, entries, categories, tags or users, but you can also eager load assets and transforms, matrix blocks. You can eager load nested related elements too, which cost me a few gray hairs already. [Straight Up Craft has an excellent guide on eager-loading](https://straightupcraft.com/articles/examples-of-eager-loading-elements-in-twig-and-php) if you want to dig deeper into it.

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
  .all() %}

{% for item in items %}
  {# display item #}
{% endfor %}
```

### The `{% cache %}` tag

[Craft's `{% cache %}` tag](https://docs.craftcms.com/v3/dev/tags/cache.html) can be used to speed up the performance of certain parts of your templates. Cached parts of a template will run needed database queries the first time a user hits the template and store the resulting html in the database. The next time a user hits that template, Craft is only going to fetch the stored HTML instead of running all those queries again.

Craft automatically clears caches when elements within `{% cache %}` and `{% endcache %}` tags are deleted or updated. You can also specify a cache duration in your templates. The default duration if you do not define one using the tag parameters is the one specified by the `[cacheDuration](https://docs.craftcms.com/v3/config/config-settings.html#cacheduration)` config setting. The default is 24 hours. You can set all caches to never expire unless elements they contain are created, updated or deleted by setting your `cacheDuration` to `false`. That behaviour will be overridden for any cache tag with a `for` parameter set to any duration.

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

Caching should be used on templates that already have been optimised, for example by using eager-loading. Caching templates or arts of templates that are performing poorly is not a viable long term solution.

That can result in big performance gains. Prime targets for the `{% cache %}` tag are:

- Long lists of entries
- Matrix fields where Matrix blocks have relational fields (entries, assets, users, categories, tags)
- Data pulled from a different site (Twitter, etc.)

You can test the efficiency of your caching strategies by making sure `devMode` is on, clearing out caches in the control panel (settings, tools) and refresh your page while looking at the Console in your browser dev tools. Look at the Profiling Summary Report, it will give you the total number of queries ran and the time it took for Craft to render the page.

The first time you refresh your page, all queries are going to run and the resulting HTML will be written to the database (in the `craft_templatecaches` table specifically). The second time you refresh the page, Craft will only pull out the cached HTML and you should see a sharp decrease in the number of queries and time to render the page.

### Front end entry forms and Guest Entries

Out of the box, Craft does allow you to create [entry forms](https://docs.craftcms.com/v3/dev/examples/entry-form.html) on the front-end of your website. These forms can only be used by registered users. You can also allow anonymous users to post entries from your front-end by using a the [Guest Entries](https://github.com/pixelandtonic/GuestEntries) first party plugin. That plugin allows you to select which sections you want to authorise guest entries for and what the default author will be for those entries.

Coupled with either an off-the-shelf notification plugin like [Sprout Email](http://sprout.barrelstrengthdesign.com/craft-plugins/email) or a custom one you wrote yourself using [events](https://craftcms.com/docs/plugins/hooks-and-events#events) and [Craft email service](https://craftcms.com/classreference/services/EmailService#sendEmail-detail), Craft makes it relatively easy to create a booking system for a free event or other similar small applications.

The syntax to use for those front-end forms is pretty simple. For the most part, the magic lies in those hidden fields.

Two important things to note:

- If there is a validation error on the entry, the URL is reloaded and an `entry` variable is made available. The `entryModel` describes the submitted entry.
- You can fetch the posted values from that `entry` variable, as well as any validation errors via `entry.getError()`, `getErrors()`, or `getAllErrors()`.

### Using Craft as a headless CMS: Element API plugin

You can also easily use Craft as a headless CMS, which is simply a CMS delivering content via an API (often a JSON API). In that scenario, your CMS does not care how the content is displayed and does not deal with the views or templates, but only with creating, updating, deleting, modifying and organising the content.

Pixel&Tonic provide an easy to use first party plugin called [Element API](https://github.com/pixelandtonic/ElementAPI) that will create API endpoints for you, ready to be consumed by your front-end layer and views (Vue.js, Ember, Angular, etc.). The [CraftQL plugin by Mark Huot]([le plugin CraftQL de Mark Huot](https://github.com/markhuot/craftql) can transform any Craft installtion in a fully functional GraphQL server.

### Importing data (import plugins)

Craft is a relatively young CMS and most projects these days are relaunches of existing websites rather than creating a website from scratch. Knowing these two facts, the need to import existing data into a new Craft install needs to be addressed in most cases.

Luckily for us, Craft has a number of great import plugins you can count on. Two of my favourite plugins are:

- [Feed Me by Verbb](https://github.com/verbb/feed-me), which lets you import XML, RSS or ATOM feeds.
- [Sprout import by Barrel Strength Design](https://github.com/barrelstrength/craft-sprout-import) lets you import JSON files.

I generally go about it by creating RSS feeds in the old install, which most CMS will let you do, and use Feed Me to import nodes as entries in Craft. Some manual work is usually needed to tidy things up but the bulk of the work can often be automated.

## Exercises

### Together

We are going to build a simple blog sporting the following pages:

1. Homepage
  - single section
  - displays only the last 3 blogposts
2. Blog archive page
  - paginated list with 5 posts on each page
  - blogposts can be filtered using categories
3. Blog detail page
  - blogposts must have images so we can learn to use image transforms
4. About page
  - single section

### On your own

1. Add the possibility to filter posts by year on the blog archive page using dynamic routing.
2. Add a "portfolio" section to present case studies. Use a Matrix field to be able to mix images and text blocks in case studies. Create links allowing you to navigate to the previous / next case study in the list.
3. Add a search tool and a search result page

## Resources

- [Official documentation](https://docs.craftcms.com/v3/) for Craft.
- [Official Help & Support articles](http://buildwithcraft.com/help) on the Craft website.
- [Craft stackexchange site](http://craftcms.stackexchange.com/): ask questions, get answers. The whole Pixel&Tonic team is on it.
- [Twig documentation](http://twig.sensiolabs.org/doc/templates.html) for template designers.
- [Awesome Craft](https://github.com/craftcms/awesome): a lengthy but awesome resources list.
