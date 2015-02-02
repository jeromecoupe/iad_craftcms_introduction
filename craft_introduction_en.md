# Craft by Pixel&Tonic: Introduction

## Craft

[Craft](http://buildwithcraft.com/) is a young and very capable CMS created by [Pixel & Tonic](http://pixelandtonic.com/). Craft is developed using open source technologies like PHP and MySQL and is based on a PHP framework with a great track record: [Yii](http://www.yiiframework.com/).

Craft focuses on the essentials: defining and managing your content in the most modular and flexible way possible. For example, localisation is built in from the get go but if you need comments, a forum or e-commerce, you will have to turn to add-ons.

### A modular pricing model

This flexibility is present into the pricing of the product itself.

- The "Core" version of Craft is free and allows you to easily develop and manage sites with a relatively simple data structure.
- The "Client" version costs $199 and allows you to use more advanced features in terms of content management but only offers you basic user management.
- The "Pro" version costs $299 and allows you to use all the features of Craft, including its advanced multilingual capabilities and granular user management.

# Strengths

In my opinion, the main strengths of Craft are:

1. An unparalleled flexibility in the definition of your item types and of their data structure. The 16 field types available by default allow for an extremely modular approach of your content.
2. Speaking of modular fields, Matrix certainly is Craft's main asset in terms of creating a flexible data structure while maintaining total control over the generated front-end code.
3. Using Twig as its templating language. Learning Twig certainly has a learning curve but the benefits in terms of power, performance, modularity and flexibility for your templates are amazing.
4. An integrated and comprehensive solution for multilingual sites with great localisation features.
5. An impressive amount of features available out of the box, designed to make your life and the life of your users easier: live preview, responsive control panel, one click updates, multiple environments configurations, etc.
6. Freedom to build your own URL structure: there is almost no relation between your URL structure and your folder and files structure, thanks to a very flexible routing mechanism.
7. A fantastic development and support team.

## Define and structure your project

Craft allows you to create a very flexible and modular data structure for your project.

### Sections, Entries and entry types

With Craft, your content will mainly live in entries. Those entries are grouped under entry types to form sections.

The data structure of those entries is created by assigning custom fields to the entry types you defined for each sections. For each of those entry types, you can create a field layout defining which fields will be used by all entries of that type in a given section.

There are [three types of sections](http://buildwithcraft.com/docs/sections-and-entries): singles, channels and structures.

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

These entry types can easily be [used in routing and URL structures](http://buildwithcraft.com/help/entry-type-urls) as well as in your templates with [`craft.entries` tags and conditionals](http://buildwithcraft.com/docs/templating/entrymodel#type).

#### Structure sections

Structure sections behave a lot like channel sections: they can contain various entry types and the URL structure of their entries can be specified. The main difference is that their entries can be manually ordered in a hierarchical tree. You can  specify the number of levels in that hierarchy.

Using the section configuration screen you can define

- The URL format for entries in that section. Different URL pattern can be specified for top-level entries and for nested entries.
- The template Craft should load to display entries belonging to that section.

For each entry type defined in your section, you can define the data structure of entries by assigning custom fields to a field layout.

### Fields, Field Groups and Field Layouts

Craft comes natively with [16 field types](http://buildwithcraft.com/docs/fields) through which you can define the data structure of your entries.

A field can be applied to several entries, users, assets, tags, categories or globals via a "field layout" allowing you to perform operations on the fields (ordering, make mandatory or not, etc) via a drag and drop interface.

Fields can be grouped into field groups. These groups are there purely for convenience sake to help you manage big projects more easily.

### Globals

Next to sections and entries, [globals](http://buildwithcraft.com/docs/globals) can be used to store bits of content or options you want your client to be able to edit easily: tagline, contact data, Google Analytics code, number of entries to display in lists, etc.

Those globals can be grouped together using global sets. Each global set has its own field layout and thus its own data structure. The big difference between global sets and single entries are that global sets do not have their own URLs and cannot make use of the "live preview" function.

### Users

Craft has [powerful and granular user management](http://buildwithcraft.com/docs/users) allowing you to manage the permissions for all users of the system.

Users can be assigned to various user groups. Permissions can be managed at a user group or at a user level. A user can be assigned to multiple groups.

The data structure for your users can be defined easily. A unique field layout allows you to assign custom fields to all your users, regardless of their groups.

### Assets

[Assets](http://buildwithcraft.com/docs/assets) allow you to manage your files with Craft (images, videos, sounds, PDFs, etc). Assets are assigned to Asset Sources corresponding to folders on your server, or on external Cloud servers like Rackspace or Amazon S3 if you are the proud owner of a Craft Pro license.

Craft allows you to apply [transformations](http://buildwithcraft.com/docs/image-transforms) (crop, fit, scale, quality, etc.) to images automatically, either in the control panel or dynamically through templates. That feature allows you to generate all the thumbnails you needs from an initial image.

A field layout is available for each Asset Source. Using it in combination with custom fields allow you to create complex data structure for each of your assets types. For example, documents can have a different data structure than photos.

### Tags

[Tags](http://buildwithcraft.com/docs/tags) allow you to create *folksonomies* and apply them to your Entries, Users or Assets.

Every tag must be assigned to a group and each tag group has a field layout. You can create complex data structures for each of your tag groups if needed.

### Categories

[Categories](http://buildwithcraft.com/docs/categories) allow you to create *taxonomies* and apply them to your Entries, Users or Assets.

Each category must be assigned to a group and each of them has a dedicated field layout.

For each category group, the edit screen allows you to:

- Specify the URL structure for categories and sub categories
- Specify the template that Craft will load to display when a category URL is requested.

### Relations

One of Craft's great strengths is its [relations system](http://buildwithcraft.com/docs/relations). You can easily create relations between Entries, Users, Assets and Tags through a series of relational field types:

- **Assets**: allows you to establish a "one to one" or "one to many" relation to Assets.
- **Entries**: allows you to establish a "one to one" or "one to many" relation to Entries.
- **Users**: allows you to establish a "one to one" or "one to many" relation to Users.
- **Tags**: allows you to establish a "one to one" or "one to many" relation to Tags.
- **Categories**: allows you to establish a "one to one" or "one to many" relation to Categories.

For each of those field, you can specify how many items can be linked and from what source(s) they come from.

To display and work with these relations in your templates, Craft is giving you a very powerful tool in the form of the [`relatedTo`](http://buildwithcraft.com/docs/relations#the-relatedTo-param) parameter. You can use that parameter with `craft.entries`, `craft.users`, `craft.assets`, `craft.tags` and `craft.categories`.

### Routing

Another interesting aspect of Craft is its [dynamic routing system](http://buildwithcraft.com/docs/routing), which allows you separate your URL structure from your folders and files structure.

At the most basic level, we have already seen that Craft allows you to specify the URL structure of each of your Entries, Users, Assets, Tags and Categories.

If that's not enough to cover all your needs, you can also create dynamic routes independently. For each route you create, you can specify which template must be loaded by Craft. An easy to understand example is [a template giving access to a yearly archive of entries](http://buildwithcraft.com/help/entry-archive#yearly-archive-pages).

When you create a dynamic route with the structure of `blog/archive/{year}` calling the template `blog/archive`, the URLs `blog/archive/2013` and `blog/archive/2012` will load the same template and make a `{year}` variable accessible to to Twig in that template. You can for example use it with the [`after`](http://buildwithcraft.com/docs/templating/craft.entries#after) and [`before`](http://buildwithcraft.com/docs/templating/craft.entries#before) parameters of a `craft.entries` tag.

If you need more possibilities than those the Control Panel gives you, you can [manage your routes in a more advanced way](http://buildwithcraft.com/docs/routing#advanced-routing) using the `config/routes.php` file. That will give you access to all the power of regular expressions in your URL matching patterns.

### Search

Craft also has a [very powerful built-in search system](http://buildwithcraft.com/docs/searching) based on a `search` parameter that can be used with `craft.entries`, `craft.users`, `craft.assets` and `craft.tags`.

For performance reasons, Craft uses indexes for its search functionalities and those indexes can be updated or [rebuilt](http://buildwithcraft.com/docs/searching#rebuilding-your-search-indexes) directly from the control panel.

It is also easy to build [dynamic search forms](http://buildwithcraft.com/docs/templating/search-form) for your project. All you need to do is to use the `craft.request` tag to be able to [use the GET/POST parameters](http://buildwithcraft.com/docs/templating/craft.request) passed by your form in the context of your results template.

## Create your templates

Now that you know how to create complex data structures for your projects, let's dive into creating your templates with Craft.

All your templates are in the `craft/templates` folder. You can change that location using the `[CRAFT_TEMPLATES_PATH](http://buildwithcraft.com/docs/php-constants#craft-templates-path)` constant.

### Twig as a templating language

Craft uses [Twig](http://twig.sensiolabs.org/), created by Fabien Potencier, as its templating language. Twig compiles all your templates down to raw PHP, which means it is blazing fast. Twig has a small learning curve if you have never done any programming but remains very [accessible to front-end developers](http://twig.sensiolabs.org/doc/templates.html).

Coupled with Craft specific tags, functions and filters, Twig enables you to access your data and manipulate them in your templates. For a quick introduction to Twig other than what follows, have a look at "[Twig for designers](http://www.slideshare.net/brandonkelly212/twig-for-designers)", a presentation by Brandon Kelly available on Slideshare.

#### Main tags in Twig

On top of the [tags available in Twig](http://twig.sensiolabs.org/doc/tags/index.html), Craft also has [a few tags of its own](http://buildwithcraft.com/docs/templating/tags). We will cover some of them in the rest of this course.

Twig has three main types of tags:

- `{# Comments #}`
- `{{ Output }}`
- `{% Execution / Logic %}`

##### Comment tags

Twig comment tags are not rendered in the compiled template: `{# This is a comment: not rendered in the compiled template #}`.

##### Output tags, variables and properties

`{{ Output }}`: These tags allow you to output strings, numbers, booleans, arrays and objects in your templates. Most of the time you will be outputting variables that you or Craft sets.

Examples:

- `{{ "Hello world" }}`: displays the string "Hello world"
- `{{ entry.title }}`: displays the title property of an entry object
- `{{ 8 + 2 }}`: displays 10

##### Execution and logic tags

`{% Execution / Logic %}`: these tags allow you to execute tasks and can be used to create variables, loops, control structures, etc.

Examples:

Create an `allEntries` variable and assign it an [ElementCriteriaModel](http://buildwithcraft.com/docs/templating/elementcriteriamodel) Craft object containing all entries in the `blog` section, ordered by creation date in descending order.

```twig
{% set allEntries = craft.entries.section('blog').limit(null).order('postDate desc').find() %}
```

Loop on the `allEntries` object to create `entry` objects and display their titles.

```twig
{% for entry in allEntries %}
	<h1>{{ entry.title }}</h1>
{% endfor %}
```

Create a [control structure](http://twig.sensiolabs.org/doc/templates.html#control-structure) to check if the `allEntries` variables contains at least one entry.

```twig
{% if allEntries|length %}
	<p>There is at least one entry here</p>
{% endif %}
```

##### Never use tags inside other tags

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

#### Data types and variables with Twig

Twig supports 5 big data types:

- Strings: `"Hello"`
- Numbers: `4` or `3.1498`
- Booleans: `true` or `false`
- Arrays: `['orange', 'lemon', 'apple']`
- Objects: `{ foo: 'bar' }`

As shown earlier, you can easily assign a value to a variable using the `{% set %}` tag

```twig
{% set firstName = "Jérôme" %}`
{% set allEntries = craft.entries.section('blog').limit(null).order('postDate desc').find() %}
```

#### Filters

[Twig](http://twig.sensiolabs.org/doc/filters/index.html) and [Craft](http://buildwithcraft.com/docs/templating/filters) have filters that can be applied to your variables and modify or manipulate them. It means that Craft doesn't need any plugin to perform simple tasks in your templates. Here are some examples of what can be done using filters:

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
{% set allEntries = craft.entries.section('blog').limit(null).order('postDate desc').find() %}
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

#### Functions

[Twig](http://twig.sensiolabs.org/doc/functions/index.html) and [Craft](http://buildwithcraft.com/docs/templating/functions) both have functions allowing you to execute functions on your data.

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

`dump()` is only accessible in Craft when [Dev Mode is enabled](http://buildwithcraft.com/help/dev-mode) in your `config/general.php` file. Essential for debugging.

```twig
{{ dump(entry) }}
```

#### Control structures and conditionals

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

#### Mathematical expressions and string manipulations

Twig knows [Math](http://twig.sensiolabs.org/doc/templates.html#math) and allows you to manipulate character strings easily using [filters](http://twig.sensiolabs.org/doc/filters/index.html).

```twig
{{ 10 * (8+2) }}
{{ "Hello World"|slice(0,5) }}
```

#### Whitespace control

Twig allows you to control how your code is displayed, including [at the whitespace level](http://twig.sensiolabs.org/doc/templates.html#whitespace-control), which, if you are using `inline-block`, is a godsend.

#### Template inheritance, includes and macros: stay DRY

These three concepts are at the heart of Twig as a templating language. They enable you to create efficient templates and avoid redundancy, allowing you to comply with the infamous "Don't Repeat Yourself" (DRY) principle.

##### Template inheritance

Template inheritance is a central concept in Twig, as it is in many other templating languages for the web. We will generally work with a "parent" template defining the skeleton of the page, its different blocks and one or more "children" templates. These children templates will extend the parent template and define the content of each of those blocks.

Variables defined in "children" templates can be accessed in the "parent" template.

Let's see how it works practically with a very simple example:

**Parent template: layouts/_base.html**

```twig
<!DOCTYPE html>
<html class="no-js" lang="en">

<head>
	<meta charset="utf-8">

	<title>{% if htmlTitle is defined %}{{ htmlTitle }}{% else %}My generic title{% endif %} - Mysite</title>

	<meta name="viewport" content="width=device-width, initial-scale=1">

	<link rel="shortcut icon" href="{{ siteUrl }}/favicon.ico">
	<link rel="stylesheet" media="screen" href="{{ siteUrl }}/assets/css/screen.css">
	<link rel="stylesheet" media="print" href="{{ siteUrl }}/assets/css/print.css">

	<script src="{{ siteUrl }}/assets/js/libs/modernizr.js"></script>

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

	{% for entry in craft.entries.section('news').find() %}
		<article>
			<h3><a href="{{ entry.url }}">{{ entry.title }}</a></h3>
			<p>Posted on {{ entry.postDate.format('F d, Y') }}</p>
			{{ entry.body }}
			<p><a href="{{ entry.url }}">Continue reading</a></p>
		</article>
	{% endfor %}

{% endblock %}
```

The `news/index.html` template extends the base layout template. The content defined in the "content" block of the child template replaces / supersedes the content of the "content" block in the parent template.

As a side note, the name of the "parent" template begins with an underscore to tell Craft that template is hidden and cannot be accessed directly via a web browser. If any segment of a URL begins with an underscore (template or template group), Craft will display a 404.

Typically, your layouts, includes and entry templates should all be hidden.

##### Includes

If you have code that is repeated in many templates, it is generally a good idea to use includes `{% include %}`. Since Twig compiles everything down to raw PHP, there is no real performance penalty for using includes.

```twig
{% include '_sidebars/default.html' %}
```

#### Macros

Twig [Macros](http://twig.sensiolabs.org/doc/tags/macro.html) are comparable to mixins in Sass. They are small reusable chunks of code.

A Macro is defined using the `{% macro %}` and `{% endmacro %}` tags. Macro can be loaded from an external file, or reside in the file they are used in.

```twig
{% macro errors(list) %}
  {% if list|length %}
    <ul class="errors">
      {% for error in list %}
        <li>{{ error }}</li>
      {% endfor %}
    </ul>
  {% endif %}
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
{% import "_macros/errors" as formErrors %}
{{ formErrors.errors(entry.allErrors) }}
```

### Retrieve and display your data with Craft

In Craft, you interact with the database using [ElementCriteriaModel](http://buildwithcraft.com/docs/templating/elementcriteriamodel) objects. It sounds complicated but it is in fact quite simple:

1. you create an ElementCriteriaModel for the type of data you want to get from the database (entries, users, assets, etc.)
2. you specify the parameters (limit, order, filters, etc.) you want to use.
3. Craft then fetches what you need from the database
4. Craft returns an ElementModel or an array of ElementModel objects ([EntryModel](http://buildwithcraft.com/docs/templating/entrymodel) for entries, [UserModel](http://buildwithcraft.com/docs/templating/usermodel) for users, [AssetFileModel](http://buildwithcraft.com/docs/templating/assetfilemodel) for assets, [CategoryModel](http://buildwithcraft.com/docs/templating/categorymodel) for categories and [TagModel](http://buildwithcraft.com/docs/templating/tagmodel) for tags).
5. You can then display those objects or arrays of objects in your template.

`craft.entries`, `craft.users`, `craft.assets`, `craft.categories` and `craft.tags` will be your main tools to retrieve and display your data.

We will mainly work with the `craft.entries` tag in this introduction. Since all tags use the same principles, it will be easy for you to apply what you know to `craft.users`, `craft.assets`, `craft.categories` and `craft.tags`.

#### Entries

[`craft.entries`](http://buildwithcraft.com/docs/templating/craft.entries) is going to be your main tool to retrieve and display your entries.

- `craft.entries.find()` allow you to retrieve all entries corresponding to the specified criteria.
- `craft.entries.total()` allow you to retrieve the total number of entries corresponding to the specified criteria.
- `craft.entries.first()`, `craft.entries.last()` and `craft.entries.nth(n)` allow you to retrieve the first, the last or the nth entry corresponding to the specified criteria.
- `craft.entries.ids()` allow you to retrieve the ids of all entries corresponding to the specified criteria.

##### Two different syntaxes

You can use two different syntaxes with Craft: a dot notation (chained parameters) or an object notation (parameters as object).

```twig
{% set allEntries = craft.entries.section('news').limit(4).find() %}

{% for entry in allEntries %}

	<h2>{{ entry.title }}</h2>
	{{ entry.summary }}

{% endfor %}
```

or


```twig
{% set allEntries = craft.entries.find({
	section:'news',
	limit:4
}) %}

{% for entry in allEntries %}

	<h2>{{ entry.title }}</h2>
	{{ entry.summary }}

{% endfor %}
```

Both syntaxes are valid and each of them has its place. The object notation is particularly useful if you need to reuse the parameters multiple times in your template. Here is an example fetching the entries themselves and the number of entries using the same parameters passed as an object.

```twig
{% set params = {
	section:'news',
	orderby:'postDate desc'
} %}

{% set totalEntries = craft.entries.total(params) %}
Total entries: {{ totalEntries }}

{% for entry in craft.entries.find(params) %}

	<h2>{{ entry.title }}</h2>
	{{ entry.summary }}

{% endfor %}
```

##### No results

You can easily display alternate content if no entries are found, just by using [an `{% else %}` clause in your `{% for %}` loop](http://twig.sensiolabs.org/doc/tags/for.html#the-else-clause).

```twig
{% set allEntries = craft.entries.section('news').limit(4).find() %}

{% for entry in allEntries %}

	<h2>{{ entry.title }}</h2>
	{{ entry.summary }}

{% else %}

	<p>No news found.</p>

{% endfor %}
```

##### "Loop", "cycle" and "is divisible by"

When using a `{% for %}` loop, it is sometimes useful to know at which iteration of the loop you are currently at. Typical tests include opening and closing a `<ul>` at the beginning and at the end of a loop, or displaying something every x iterations. Twig gives you the [`loop`](http://twig.sensiolabs.org/doc/tags/for.html#the-loop-variable) variable, the [`cycle`](http://twig.sensiolabs.org/doc/functions/cycle.html) function and the [`is divisibleby`](http://twig.sensiolabs.org/doc/tests/divisibleby.html) test for those jobs.

**Example**: using the `loop` variable

```twig
{% set allEntries = craft.entries.section('news').limit(4).find() %}


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
{% set allEntries = craft.entries.section('news').limit(4).find() %}


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
{% set allEntries = craft.entries.section('news').limit(4).find() %}

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

##### Pagination

Craft allows you to [paginate your results](http://buildwithcraft.com/docs/templating/tags#paginate) using the `{% paginate %}` tag and to build simple or more complex pagination interfaces using the related variables. One small caveat: the `{% paginate %}` tag needs an ElementCriteriaModel as parameter. Just don't call `find()` on the object.

```twig
{% paginate craft.entries.section('news').limit(5) as entries %}

	{# get paginated entries #}
	{% for entry in entries %}
		<article>
			<h3 class="title-item"><a href="{{ entry.url }}">{{ entry.title }}</a></h3>
			<p class="meta-info">Posted on {{ entry.postDate.format('F d, Y') }}</p>
			{{ entry.body }}
			<p><a href="{{ entry.url }}">Read More</a></p>
		</article>
	{% endfor %}

	{# Build pagnation interface if more than 1 page #}
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

{% endpaginate %}
```

##### Detail page and "entry" variable

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

##### Category page and "category" variable

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
	#	- craft automatically creates a 'category' variable if it detects you are on a category template
	#  - we are just checking whether that category variable exists or not
	#  - depending on its existence, we set our list of entries
	#}

	{% set allCategories = craft.categories.group('newsTopics').find() %}

	{% if category is defined %}
		{% set currentCategory = category.slug %}
		{% set allNews = craft.entries.section('news').relatedTo(category).limit(10) %}
	{% else %}
		{% set currentCategory = 'all' %}
		{% set allNews = craft.entries.section('news').limit(10) %}
	{% endif %}

	{# display entries list #}
	{% paginate allNews as entries %}

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

	{% endpaginate %}

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

#### Globals

Globals are used to store content that will be made globally accessible to all your templates. They are typically used to create configuration options or variables that need to be editable by administrators but that do not belong in sections.

Globals can be accessed easily via their global set handle followed by their global handle. For example, a global called `tagline` in a `companyInfo` global set would be accessed this way:

`{{ companyInfo.tagline }}`

#### Tags

You can access and display tags using `craft.tags` and its [related parameters](http://buildwithcraft.com/docs/templating/craft.tags). It works just like `craft.entries` but returns a `[TagModel](http://buildwithcraft.com/docs/templating/tagmodel)` object or an array of those.

Articles in the help section are showing you how to list [all the tags used by the entries in a given section](http://buildwithcraft.com/help/active-tags) or how use a dynamic route to create an [archive page listing all the entries related to a tag](http://buildwithcraft.com/help/tag-urls).

#### Users

`craft.users` allows you to access and display the users of your website. The tag functions like `craft.entries` but returns a single `[UserModel](http://buildwithcraft.com/docs/templating/usermodel)` or an array of those. The `craft.users` tag also has [a series parameters](http://buildwithcraft.com/docs/templating/craft.users), some of which are tied to users-specific functionalities or behaviours.

#### Assets and transformations

The `craft.assets` tag will allow you to access your assets. This tag also has a series of parameters, some of which are tied to assets-specific functionalities or behaviour. `craft.assets` functions like `craft.entries` except that it returns a single `[AssetFileModel](http://buildwithcraft.com/docs/templating/assetfilemodel)` or an array of those.

If your assets are images, Craft allows you to create transforms tied to all your asset groups. These transforms will generate thumbnails for all your assets. Those transforms can be specified in the control panel and generated when assets are uploaded (Settings > Assets > Image Transforms) or they can be specified in your template and generated dynamically when assets are requested for the first time.

When you define a transform in the Control Panel and you name it `thumbnail`, you can access them in your templates in the following way:

```twig
{% if myAssetField | length %}
	{% set heroImage = myAssetField.first() %}
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

{% if myAssetField | length %}
	{% set heroImage = myAssetField.first() %}
	<img src="{{ heroImage.getUrl(transform) }} width="{{ asset.getWidth(transform) }}" height="{{ asset.getHeight(transform) }}" alt="{{ heroImage.title }}">
{% endif %}
```

## Slightly more advanced functionalities

### Redactor configurations

Craft uses [Redactor](http://imperavi.com/redactor/) as a WYSIWYG solution for rich text fields. When you create a rich text field, you can choose the Redactor configuration you want to apply to that field.

These configurations can be easily created and modified with JSON files stored in the `craft/config/redactor/` folder. The name of your JSON file will be the name of the configuration in the Control Panel.

### Multiple environments configurations

Craft offers a native way to deal with multiple environments configurations (dev, staging, online) through the use of nested arrays and [environment-specific variables ](http://buildwithcraft.com/docs/multi-environment-configs) in your `general.php` and `db.php` configuration files.

First, define a `*` array. Values specified in there will be applied to all your environments. The `*` array is mandatory, even if it contains nothing, as Craft looks for it to enable multi-environment config support.

The rest of your array keys will reference domain names or parts thereof on your different servers. Craft will check those keys against the `$_SERVER['SERVER_NAME']` PHP variable an look for a (partial) match.

**Example**: `craft/config/general.php`

```
return array(
    '*' => array(
        'omitScriptNameInUrls' => true,
    ),

    'domain.dev' => array(
        'devMode' => true,
    ),

    'domain.com' => array(
        'cooldownDuration' => 0,
    )
);
```

You can pretty much override any [configuration settings](http://buildwithcraft.com/docs/config-settings) that way. Craft also allows you to create and use [environment specific variables](http://buildwithcraft.com/docs/multi-environment-configs#environment-specific-variables). You can name those any way you want and use them to create dynamic configurations in Craft's control panel. You can also [use them in your templates](http://buildwithcraft.com/docs/templating/craft.config) using the `craft.config.myvariable` syntax

```
return array(
    '*' => array(
        'omitScriptNameInUrls' => true,
    ),

    'domain.dev' => array(
        'devMode' => true,

        'environmentVariables' => array(
            'rootUrl'        => 'http://www.domain.dev/',
            'serverPath' => '/users/sitename/web/'
            'cpTrigger'      => 'adminpanel',
        )
    ),

    'domain.com' => array(
        'cooldownDuration' => 0,

        'environmentVariables' => array(
            'rootUrl'        => 'http://www.domain.com/',
            'serverPath' => '/users/domain/htdocs/'
            'cpTrigger'      => 'adminpanel',
        )
    )
);
```

You can then use those variables in the Control Panel, for example when defining file system paths and url for your asset sources to make them environment specific.

```
{serverPath}assets/images/
{rootUrl}assets/images/
```

Such dynamic configurations can also be used for your database parameters in `craft/config/db.php`.

**Example**: craft/config/db.php

```
return array(
    '*' => array(
        'tablePrefix' => 'craft',
    ),

    'domain.dev' => array(
        'server' => 'localhost',
        'user' => 'root',
        'password' => 'password',
        'database' => 'webstoemp',
    ),

    'domain.com' => array(
        'server' => 'localhost',
        'user' => 'myownuser',
        'password' => 'strongpassword',
        'database' => 'webstoemp',
    ),
);
```

### Matrix

Matrix certainly is one of the most interesting field type available with Craft. Essentially, it allows you to define a data structure for several types of content blocks (using Craft's other field types) and then to combine and arrange these blocks of content.

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

At the template level, you stay fully in control of the generated HTML code. We are using a special `{% switch %}` tag available for Craft.

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

			{% set image = module.imageFile.first() %}
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
2. Add a "portfolio" section to present case studies. Use a Matrix field to be able to mix image and text in case studies. Create links allowing you to navigate to the previous / next case study in the list.

## Resources

- [Official documentation](http://buildwithcraft.com/docs/introduction) for Craft.
- [Official Help & Support articles](http://buildwithcraft.com/help) on the Craft website.
- [Craft stackexchange site](http://craftcms.stackexchange.com/): ask questions, get answers. The whole Pixel&Tonic team is on it.
- [Craft on Google Plus](https://plus.google.com/communities/106505340287442511226): Craft community, monitored by the fine folks at Pixel&Tonic.
- [Twig documentation](http://twig.sensiolabs.org/doc/templates.html) for template designers.
- [Screencast by Mijingo](https://mijingo.com/products/screencasts/craft-cms-tutorial/): a very good intro for visual learners.
- [Straight up Craft](http://straightupcraft.com/): A goldmine of Craft resources, a directory of existing plugins and a list of active developers.
- [Essential videos](http://straightupcraft.com/learn-craft-cms) on Straight up Craft.
- [On the Rocks](https://github.com/pixelandtonic/ontherocks): a demo website available on Github. Good learning resource for templating.
- [CTRL+CLICK CAST](http://ctrlclickcast.com/episodes/crafty-sites-with-brandon-kelly): Craft episode with Brandon Kelly
- [Interview with Brandon Kelly](http://www.thenerdary.net/post/48123188844/interview-with-brandon-kelly-creator-of-craft) on the Nerdary.
- [Craft CMS: The (very) basics of templating](http://withchief.com/blog/basics-of-templating-in-craft) by [Jamie Pittock](https://twitter.com/jamiepittock).
- [Craft Your Content With Markdown And Matrix](http://experiencehq.net/blog/craft-with-markdown-and-matrix) by [Stephen Lewis](https://twitter.com/monooso "The undisputed king of title attributes"): very instructive templating examples.
- [Craft Cookbook](http://www.craftcookbook.net): another good problem solving resource for Craft by [Stephen Lewis](https://twitter.com/monooso "Yes, him again") and the community.
- [Making Sense of Twig](http://www.slideshare.net/brandonkelly212/twig-for-designers): by [Brandon Kelly](https://twitter.com/brandonkelly) a well rounded introduction to Twig as a templating language.
- [Real World Craft Tips & Tricks](https://speakerdeck.com/trevor_davis/real-world-craft-tips-and-tricks): a slidedeck by [Trevor Davis](https://twitter.com/trevor_davis). A nice collection of tips and tricks for Twig and Craft.
- [Manipulating Craft's ElementCriteriaModel objects with Twig](http://www.webstoemp.com/blog/manipulating-craft-elementcriteriamodel-with-twig/): a simple but powerful technique to build complex functionalities with only a few lines of Twig.
