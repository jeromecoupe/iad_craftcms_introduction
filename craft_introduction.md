# Craft by Pixel&Tonic: Introduction

## Craft

[Craft](http://buildwithcraft.com/) est un CMS récent créé par [Pixel & Tonic](http://pixelandtonic.com/). Craft est développé à l'aide de technologies open source comme PHP et MySQL et s'appuie sur un framework PHP qui a fait ses preuves: [Yii](http://www.yiiframework.com/).

Comparé à d'autres produits, Craft est axé sur l'essentiel: définir et gérer vos contenus, et ce de la façon la plus modulaire et flexible possible. Si vous avez besoin d'un moteur de commentaires, d'un forum ou encore d'e-commerce, il faudra vous tourner vers des plugins.

### Un modèle de pricing modulaire

Cette flexibilité est présente jusque dans [le pricing](http://buildwithcraft.com/pricing) du produit lui même.

- La version "Core" de Craft est gratuite et vous permet de développer facilement des sites assez simples en terme de data structure.
- Une version "Client" à 199$ vous permet de disposer de fonctionnalités plus avancées et d'une gestion utilisateurs basique.
- Une version "Pro" à 299$ vous permet de disposer de toutes les fonctionnalités de Craft, y compris ses fonctionnalités multilingue et de gestion d'utilisateurs.

### Atouts

Les principaux atouts de Craft sont à mon sens:

- Une flexibilité dans la définition de votre data structure rarement égalée. Les 16 field types disponibles par défaut permettent une approche extrêmement modulaire.
- A propos de champs et de modularité, Matrix est véritablement un atout de Craft en ce qui concerne la flexibilité de vore data structure tout en préservant votre contrôle sur le code front-end généré.
- [L'utilisation de Twig comme language de templating](http://twig.sensiolabs.org/doc/templates.html): cela induit un temps d'apprentissage mais les gains en termes de puissance, de modularité et de flexibilités sont étonnants. Toute la puissance et la maturité de Twig sont au services de vos templates.
- Une quantité impressionnantes de fonctionnalités qui rendront la vie de vos clients plus facile: live preview, control panel responsive, one click updates, etc.
- Un découplage important entre votre structure de dossiers et de fichiers et la manière dont sont construites les URL. La structure de vos URL est donc extrêmement flexible.
- Une solution intégrée et complète pour les sites multilingues.
- Une équipe de développement et de support fantastique

## Définir et structurer votre projet

Comme dit plus haut, Craft vous permet de structurer les données de votre site de façon extrêmement flexible et modulaire.

### Sections, Entries et entry types

Dans Craft, vos contenus vont principalement "vivre" dans des entries, elles-même contenues dans des sections.

La data structure de ces entries va être déterminée par les custom fields que vous ajouterez à ces sections. Pour chaque entry type dans Craft, vous pouvez créer un field layout qui va préciser quels fields vont être utilisés pour définir les entries de cette section.

Il y a [trois grands types de sections](http://buildwithcraft.com/docs/sections-and-entries) dans craft: singles, channels et structures.

#### Sections de type single

Ces sections contiennent un seul entry type et une seule entry. Elles sont utilisées pour les pages particulières de votre site, comme par exemple une page "about" ou votre "homepage".

Via l'écran de configuration de la section vous pouvez préciser:

- le format de URL de votre unique entry
- le template à utiliser pour le rendu de l'entry

Pour l'entry type disponible vous pouvez spécifier un field layout: assigner à votre entry type des custom fields qui vont en définir la data-structure

#### Sections de type channel

Ces sections contiennent un ensemble d'entries que vous pouvez ensuite classer et afficher de diverses façons à l'aide de vos templates.

Via l'écran de configuration de la section vous pouvez préciser:

- le format des URL des entries de la section
- le template à utiliser pour le rendu des entries de la section

Les sections de type channel peuvent contenir différents types d'entries ("entry types") avec des data structure différentes. Créer un blog permettant de poster divers types de contenus est donc très facile. Vous n'avez besoin que d'une seule section avec différents types d'entrées (video, post, son, galerie photo, etc.).

Pour chaque entry type disponible vous pouvez spécifier un field layout: assigner à votre entry type des custom fields qui vont définir la data-structure de toutes les entries de ce type

L'entry type peut facilement être utilisé [dans le routing et les patterns d'URL](http://buildwithcraft.com/help/entry-type-urls), comme [dans vos tags craft.entries et vos tests conditionnels](http://buildwithcraft.com/docs/templating/entrymodel#type)

#### Sections de type structure

Les sections de type structure ressemblent beaucoup aux sections de type channel: elles peuvent contenir plusieurs entry types et les patterns d'URL des entries qu'elles contiennent peuvent être précisés.

La grande différence est que ces sections de type structure sont destinées à créer des agencements hiérarchiques d'entries dont l'ordre peut être modifié manuellement. Vous pouvez également préciser le nombre de niveaux possibles dans cette hiérarchie d'entries, les patterns d'URL à utiliser suivant les niveaux des entries dans la hierarchie, ainsi que le template à utiliser pour les entries de la section.

Via l'écran de configuration de la section vous pouvez préciser:

- le format des URL des entries de la section. Ce format peut être différent pour les entrées de niveau 1 ou pour les entries imbriquées.
- le template à utiliser pour le rendu des entries de la section

Pour chaque entry type disponible vous pouvez spécifier:

- un field layout permettant d'assigner à vos entry type des custom fields qui vont définir la data-structure de toutes les entries de ce type

### Fields, Field Groups et Field Layouts

Craft vous propose [16 types de champs](http://buildwithcraft.com/docs/fields) à l'aide desquels vous pouvez définir la data structure de vos entries.

Dans Craft, un champ peut être appliqué à n'importe quel nombre d'entries, de users, de tags, d'assets, de categories ou de globals via un "field layout" qui permet d'effectuer toutes les opérations sur les fields dans une interface "drag and drop".

Les fields peuvent être groupés au sein de groupes. Ces groupes n'ont qu'une fonction organisationnelle. Créer des groupes permet de gérer plus facilement un grand nombre de champs.

### Globals

A côté des sections et des entries, les globals peuvent être utilisées pour stocker du contenu auquel il est possible d'accéder dans vos templates: tagline, coordonnées de contact, code Google Analytics, etc.

Vous pouvez créer des groupes de contenus en utilisant les global sets. Chaque set de globals possède son field layout et donc sa propre data structure. Les différences notables avec les sections et les entries sont que les globals ne possèdent pas d'URL et ne peuvent donc pas être visualisées comme les entries à l'aide de la fonction "live preview".

### Users

Dans Craft, les utilisateurs servent à gérer les permissions données aux divers utilisateurs du système.

Les utilisateurs peuvent être assignés à divers groupes. Ces groupes sont utilisés pour gérer les permissions données aux utilisateurs qui en sont membres. Un utilisateur peut être assigné à plusieurs groupes.

La data structure des utilisateurs peut être aussi complexe que souhaitée. Via un unique Field layout, les champs souhaités peuvent être assignés à la data structure de tous vos utilisateurs.

### Assets

Les assets vous permettent de gérer vos fichiers Craft. Vos Assets sont enregistrés dans des Asset Sources qui correspondent à des dossiers physiques sur votre serveur.

De multiples transformations d'images peuvent être réalisées sur vos assets: crop, scale, quality, etc. Ces transformations vous permettent de créer vos thumbnails pour l'ensemble de votre site à partir d'une image de base.

Un field layout est associé à chaque Asset Source et vous permet de créer une data-structure en y d'associant des custom fields. Vos documents peuvent donc avoir une data-structure différente de vos photos par exemple.

### Tags

Les tags vous permettent de créer des *folksonomies* et de les appliquer à vos Entries, Users ou Assets.

Chaque tag doit être assigné à un groupe et chaque groupe de tags possède un field layout. Vous pouvez donc créer des structures de données assez complexes pour chacun de vos groupes de tags.

### Categories

Les categories vous permettent de créer des taxonomies et de les appliquer à vos Entries, Users ou Assets.

Chaque catégorie doit être assignée à un groupe.

- Chaque groupe possède un field layout.
- Au sein de chaque groupe vous pouvez spécifier la structure des URLs pour vos catégories et vos sous-categories
- Pour chaque groupe de categories, vous pouvez spécifier le template qui va être chargé quand une URL de catégorie est demandée.

### Relations

L'une des grandes forces de Craft c'est qu'il est possible très facilement de créer des relations entre Entries, Users, Assets et Tags.

Pour cela, Craft met à votre disposition des champs relationnels spécifiques:

- **Assets**: permet d'établir une relation "one to one" ou "one to many" vers des Assets
- **Entries**: permet d'établir une relation "one to one" ou "one to many" vers des Entries
- **Users**: permet d'établir une relation "one to one" ou "one to many" vers des Users
- **Tags**: permet d'établir une relation "one to one" ou "one to many" vers des Tags
- **Categories**: permet d'établir une relation "one to one" ou "one to many" vers des Catégories

Pour chacun des ces tags, vous pouvez spécifier combien d'items peuvent être liés et de quelles sources ils proviennent.

Pour exploiter ces relations dans vos templates, Craft met à votre disposition un outil extrêmement puissant: le paramètre [`relatedTo`](http://buildwithcraft.com/docs/relations#the-relatedTo-param), utilisable avec `craft.entries`, `craft.users`, `craft.assets`, `craft.tags` et `craft.categories`.

### Routing

L'un des autres éléments intéressant de Craft c'est le routing dynamique, qui permet de séparer structure des URLs et architecture de dossiers et de fichiers.

Comme nous l'avons déjà vu, Craft cous permet de spécifier une structure d'URL propre pour chaque Entry, User, Asset, Tags and Categories.

Si cela ne suffit pas à couvrir tous vos besoins, vous pouvez égalment créer des routes dynamiques. Pour chaque route créée, vous pouvez spécifier quel template doit être chargé par Craft.

Il est possible créer des routes et de spécifier quel template doit être chargé par Craft. Un exemple facile à comprendre est [un template donnant accès à une archive des entries classées par année](http://buildwithcraft.com/help/entry-archive#yearly-archive-pages).

En créant une route dynamique `blog/archive/{year}` renseignant le template `blog/archive`, les URL `blog/archive/2013` et `blog/archive/2012` vont charger le même template et rendre disponible une variable `year` utilisable par Twig et par Craft avec les paramètres [`after`](http://buildwithcraft.com/docs/templating/craft.entries#after) et [`before`](http://buildwithcraft.com/docs/templating/craft.entries#before).

Si vous avez besoin de plus de possibilités que celles offertes par le control panel, sachez que [les routes peuvent également être gérées via un fichier](http://buildwithcraft.com/docs/routing#advanced-routing) `config/routes.php`, ce qui vous donne accès à du matching d'URL en utilisant des expressions régulières.

### Recherche

Craft possède également un [système de recherche très puissant](http://buildwithcraft.com/docs/searching) basé sur un paramètre `search` utilisable avec les tags `craft.entries`, `craft.users`, `craft.assets` et `craft.tags`.

Pour des questions de performance, Craft fonctionne avec des Index pour ses fonctions de recherche. Il est possible d'actualiser ces Index directement depuis le Control Panel.

La construction de [formulaires de recherche dynamiques pour le front-end](http://buildwithcraft.com/docs/templating/search-form) de votre projet est également très simple. Il suffit pour cela de faire appel au tag `[craft.request]` pour [récupérer un paramètre passé en GET/POST](http://buildwithcraft.com/docs/templating/craft.request) par votre formulaire.

## Créer vos templates

Maintenant que vous savez comment créer une structure de données pour votre projet dans Craft, passons à la création des pages de votre site à l'aide des templates.

Vos templates sont localisés dans le dossier `craft/templates` de votre installation

### Twig comme language de templating

Craft utilise [Twig](http://twig.sensiolabs.org/), créé par Fabien Potencier pour Symfony, comme language de templating. Twig a l'avantage de compiler les templates en PHP, ce qui lui permet d'être très performant. C'est un language qui reste également assez simple d'approche, même si une période d'apprentissage existe.

Couplé à des tags, fonctions et filtres spécifiques à Craft, Twig vous permet de récupérer et de manipuler vos données au sein de vos templates. Pour une introduction rapide à Twig, vous pouvez [consulter les slides de Brandon Kelly sur Slideshare](http://www.slideshare.net/brandonkelly212/twig-for-designers).

#### Principaux tags dans Twig

En plus des [tags disponibles dans Twig](http://twig.sensiolabs.org/doc/tags/index.html), [Craft possède également quelques tags qui lui dont propres](http://buildwithcraft.com/docs/templating/tags). Nous reviendrons sur certains d'entre-eux dans la suite du cours.

Twig possède trois grands types de tags:

- `{# Commentaires #}`
- `{{ Affichage }}`
- `{% Execution / Logique %}`

##### Tags de commentaires

Twig possède également un tag de commentaire: `{# Ceci est un commentaire #}`. Ceux-ci ne sont pas affiché lorsque le template est affiché.

##### Tags d'affichage, variables et propriétés

`{{ Affichage }}`: ces tags vous permettent d'afficher des chaines de caractère, nombres, booléens, tableaux et objects dans vos templates. La plupart du temps, vous afficherez des variables créées par vous ou par Craft.

Une notation pointée permet d'accéder aux propriétés de ces variables.

These tags allow you to output strings, numbers, booleans, arrays and objects to the browser. Most of the time you will be outputting variables that you or Craft sets. When those variables are objects, you access their properties using a dot notation.

Exemples:

- `{{ "Hello World" }}`: affiche la chaîne de caractères "Hello World"
- `{{ entry.title }}`: affiche la propriété title de l'objet entry
- `{{ 8 + 2 }}`: affiche 10

##### Tags d'exécution ou de logique

`{% Execution / Logique %}`: ces tags permettent l'exécution de tâches et sont utilisés pour créer des variables, réaliser des boucles, créer des structures de contrôle, etc.

Exemples:

Créer une variable "allEntries" à laquelle est assignée un objet Craft [ElementCriteriaModel](http://buildwithcraft.com/docs/templating/elementcriteriamodel) contenant toutes les entries dans la section blog, classées par date de création en ordre descendant.

```twig
{% set allEntries = craft.entries.section('blog').limit(null).order('postDate desc').find() %}
```

Boucler sur l'ensemble des entries en créant à chaque fois un objet "entry" dont nous affichons le titre.

```twig
{% for entry in allEntries %}
	<p>{{ entry.title }}</p>
{% endfor %}
```

Créer une [structure de contrôle](http://twig.sensiolabs.org/doc/templates.html#control-structure) vérifiant si la variable "allEntries" contient au minimum une entry.

```twig
{% if allEntries|length %}
	<p>There is at least one entry here</p>
{% endif %}
```

##### Jamais de tags à l'intérieur d'autres tags

Dans Twig, ceci est incorrect

```twig
{{ "Hello {{ firstname }}" }}
{% set total = {{ itemPrice }} * {{ itemsNbr }} %}
```

Il vous faudra utiliser les syntaxes suivantes

```twig
{{ "Hello " ~ firstname }}
{% set total = itemPrice * itemsNbr %}
```

#### Types de données et variables dans Twig

Twig supporte 5 grands types de données:

- Strings: `"Hello"` ou `'Hello'`
- Numbers: `4` ou `3.1498`
- Booleans: `true` ou `false`
- Arrays: `['orange', 'lemon', 'apple']`
- Objects: `{ foo: 'bar' }`

Comme dit plus haut, vous pouvez assigner une valeur à une variable dans Twig en utilisant le tag `{% set %}`

```twig
{% set firstName = "Jérôme" %}`
{% set allEntries = craft.entries.section('blog').limit(null).order('postDate desc').find() %}
```

#### Filtres

[Twig](http://twig.sensiolabs.org/doc/filters/index.html) et [Craft](http://buildwithcraft.com/docs/templating/filters) possèdent des filtres qui peuvent être appliqués à vos différents types de variables pour les modifier. Ces filtres offrent de nombreuses possibilités et permettent à Craft de se passer de nombreux plugins pour effectuer des tâches simples. Voici quelques exemples de ce qu'il est possible d'accomplir:

Convertir une string en title case

```twig
{{ entry.title|title }}
```

Réaliser des opérations sur les dates et les formatter

```twig
{{ entry.postDate|date_modify("+1 day")|date("m/d/Y") }}
```

Déterminer la longueur d'une string, d'un array ou d'un objet

```twig
{% set allEntries = craft.entries.section('blog').limit(null).order('postDate desc').find() %}
{{ allEntries|length }}
```

Vous pouvez également appliquer des filtres à plusieurs lignes de votre template et pas seulement à une variable.

```twig
{% filter upper %}
    This text becomes uppercase
{% endfilter %}
```

Ces filtres peuvent également être combinés

```twig
{{ "Hello World"|upper|slice(0,5) }}
```

#### Fonctions

Les [fonctions disponibles dans Twig](http://twig.sensiolabs.org/doc/functions/index.html) permettent de produire et de manipuler des contenus. Craft possède également [ses propres fonctions](http://buildwithcraft.com/docs/templating/functions), en plus de celles fournies par Twig.

```twig
{% for entry in allEntries %}
	<p class="{{ cycle(['odd', 'even'],loop.index0) }}">{{ entry.title }}</p>
{% endfor %}
```

```twig
{{ min([1, 2, 3]) }}
{{ max([1, 2, 3]) }}
{{ random(10) }}
```

`dump()` est une fonction extrêmement utile (et seulement disponible en Dev Mode dans Craft). Essentiel pour le debugging.

```twig
{{ dump(entry) }}
```

#### Structures de contrôle et conditionnels

Twig vous permet de créer des structures de contrôles complexes à l'aide de conditionnels.

```twig
{% if allEntries|length %}
	<p>There is at least one entry here</p>
{% endif %}
```

If / else

```twig
{% if userGender == "female" %}
	<p>Hello, how are you doing?<p>
{% else %}
	<p>Hey dude, how are things?</p>
{% endif %}
```

Conditions imbriquées

```twig
{% if user %}
  {% if user.male %}
    <p>Hey there handsome!</p>
  {% else %}
    <p>Hey pretty lady!</p>
  {% endif %}
{% elseif username %}
  <p>Is this {{ username }}?</p>
{% else %}
  <p>Have we met?</p>
{% endif %}
```

Conditions cumulées

```twig
{% if user and user.male %}
{% if superAdmin or admin %}
```

Loop

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

#### Expressions Mathématiques et manipulation de chaînes de caractères

Twig est capable d'interpréter toutes sortes d'[opérations mathématiques](http://twig.sensiolabs.org/doc/templates.html#math) et de manipuler des chaînes de caractères (strings).

```twig
{{ 10 * (8+2) }}
{{ "Hello World"|slice(0,5) }}
```

#### Contrôle du whitespace

Twig vous permet de contrôler la façon dont votre code est affiché, [particulièrement au niveau du whitespace](http://twig.sensiolabs.org/doc/templates.html#whitespace-control).

#### Template inheritance, includes et macros: stay DRY

Ces trois concepts sont au coeur de Twig et vont nous permettre de créer des templates efficaces et évitant au maximum les redondances inutiles. C'est le fameux principe du "Don't Repeat Yourself" (DRY).

##### Template inheritance

Un concept central à comprendre dans Twig est celui d'héritage au niveau des templates. Dans Twig on va généralement travailler avec un template "parent" qui défini le squelette de la page et différents blocs pouvant être surdéterminés par un template "enfant" qui étend ce template parent.

Les variables définies dans le template "enfant" sont accessibles dans le template parent.

Voyons voir comment cela fonctionne dans la pratique avec un exemple simple:

**Template parent: layouts/_base.html**

```twig
<!DOCTYPE html>
<html class="no-js" lang="en">

<head>
	<meta charset="utf-8">

	<title>{% if htmlTitle is defined %}{{ htmlTitle }} - Mysite{% else %}My generic title{% endif %}</title>

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

**Template enfant: news/index.html**

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

Le template `news/index.html` étend notre layout de base. Le contenu défini dans le block "content" du template enfant remplace celui qui est (de façon optionnelle) défini dans le template parent. La variable `htmlTitle` définie dans le template enfant est accessible dans le template parent.

Remarquez également que nous écrivons le nom du template "parent" précédé par un underscore. Craft nous permet ainsi de cacher certains templates que nous ne voulons pas voir chargé directement par les visiteurs du site.

##### Includes

Si vous avez du code qui est répété dans beaucoup de vos templates, vous pouvez également utiliser le tag `{% include %}` qui vous permet d'inclure un template au sein d'un autre.

```twig
{% include 'sidebars/_default.html' %}
```

#### Macros

[Les Macros](http://twig.sensiolabs.org/doc/tags/macro.html) dans Twig sont comparables à des mixins en Sass. Pensez à elles comme à de petits blocs de code réutilisables.

Une macro est définie à l'aide des tags `{% macro %}` et `{% endmacro %}`, soit dans un fichier externe, soit dans le même fichier dans lequel elle est utilisée.

```twig
{% macro errors(list) %}  {% if list|length %}    <ul class="errors">      {% for error in list %}        <li>{{ error }}</li>      {% endfor %}    </ul>  {% endif %}{% endmacro %}```

Les macros sont appelées / importées à l'aide du tag `{% import %}`

Si la macro est définie dans le même fichier

```twig
{% import _self as formErrors %}{{ formErrors.errors(entry.allErrors) }}```

Si la macro est définie dans un fichier extérieur

```twig
{% import "_macros/errors" as formErrors %}{{ formErrors.errors(entry.allErrors) }}```
### Récupérer vos données avec Craft

Avec Craft, vous intéragissez avec la base de données en utilisant des objets ElementCriteriaModel. Cela à l'air très compliqué mais c'est en fait un concept assez simple:

1. vous créez un objet ElementCriteriaModel pour le type de données que vous souhaitez récupérer dans la base de données (entries, users, assets, etc.)
2. vous spécifiez les paramètres à utiliser (limit, order, filters, etc.)
3. Craft va chercher les données correspondant à vos critère dans la base de données
4. Craft retourne un objet ElementModel ou un array d'objets ElementModel ([EntryModel](http://buildwithcraft.com/docs/templating/entrymodel) for entries, [UserModel](http://buildwithcraft.com/docs/templating/usermodel) for users, [AssetFileModel](http://buildwithcraft.com/docs/templating/assetfilemodel) for assets, [CategoryModel](http://buildwithcraft.com/docs/templating/categorymodel) for categories and [TagModel](http://buildwithcraft.com/docs/templating/tagmodel) for tags).
5. Vous pouvez ensuite afficher ces objets ou arry d'objets dans vos templates.

`craft.entries`, `craft.users`, `craft.assets`, `craft.categories` et `craft.tags` seront vos principaux outils de travail. Nous nous centrerons ici principalement sur `craft.entries`. Les autres tags ayant un fonctionnement très similaire, il vous sera facile d'appliquer les mêmes principes.

#### Entries

[`craft.entries`](http://buildwithcraft.com/docs/templating/craft.entries) est le tag que vous allez utiliser pour récupérer vos entries.

- `craft.entries.find()` vous permet de récupérer toutes les entries qui correspondent à vos critères
- `craft.entries.total()` vous permet de récupérer le nombre total des entries correspondant à vos critères
- `craft.entries.first()`, `craft.entries.last()` et `craft.entries.nth(n)` vous permettent de récupérer la première, la dernière ou la nième des entries correspondant à vos critères
- `craft.entries.ids()` vous permet de récupérer la liste des ids des entries correspondant à vos critères

##### Plusieurs façons de faire

Vous pouvez utiliser deux syntaxes dans Craft. Une syntaxe pointée ou une syntaxe passant l'ensemble des paramètres comme un seul objet.

```twig
{% set allEntries = craft.entries.section('news').limit(4).find() %}


{% for entry in allEntries %}

	<h2>{{ entry.title }}</h2>
	{{ entry.summary }}

{% endfor %}
```

ou


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

Les deux syntaxes sont valides et chacune ont leur place. La seconde méthode est plus utile si vous devez réutiliser les paramètres plusieurs fois dans votre template.

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

##### Pas de résultats

Vous pouvez facilement afficher un contenu alternatif si aucun résultat n'est trouvé en [utilisant une clause {% else %} dans votre loop](http://twig.sensiolabs.org/doc/tags/for.html#the-else-clause).

```twig
{% set allEntries = craft.entries.section('news').limit(4).find() %}


{% for entry in allEntries %}

	<h2>{{ entry.title }}</h2>
	{{ entry.summary }}

{% else %}

	<p>No news found.</p>

{% endfor %}
```

##### "Loop", "cycle" et "is divisible by"

Lorsque une boucle `{% for %}` est utilisée, il est souvent très pratique de pouvoir évaluer à quelle étape de la boucle on se trouve et d'utiliser des conditionnels. Typiquement, il est utile de pouvoir ouvrir et fermer une liste `<ul>` en début ou en fin de loop, de pouvoir afficher quelque chose tous les x résultats. C'est à cela que servent la variable [`loop`](http://twig.sensiolabs.org/doc/tags/for.html#the-loop-variable), la fonction [`cycle`](http://twig.sensiolabs.org/doc/functions/cycle.html) et le test [`is divisibleby`](http://twig.sensiolabs.org/doc/tests/divisibleby.html) de Twig.

**Exemple**: utilisation de la variable `loop`

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

**Exemple**: utilisation de la fonction `cycle` pour ajouter des classes `odd` et `even`. Remarquez l'usage de `loop.index0` pour avoir une itération indexée sur zéro et non sur 1 comme avec `loop.index`.

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

**Exemple**: utilisation du test `is divisible by` pour insérer un élément toutes les 2 itérations.

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

Les [autres tests disponibles avec Twig](http://twig.sensiolabs.org/doc/tests/index.html) valent également la peine d'être consultés, notamment le test `is defined`. Les tests disponibles dans Twig sont:

- `is constant`- `is defined`- `is divisible by`- `is empty`- `is even`- `is iterable`- `is null`- `is odd`- `is same as`

##### Pagination

Craft vous permet de [paginer vos résultats](http://buildwithcraft.com/docs/templating/tags#paginate) à l'aide du tag `paginate` et de construire une interface de pagination à l'aide des variables qui l'accompagnent.

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

##### Page de détail et variable "entry"

Lorsque Craft charge un template de détail et que l'URL correspond à l'URI d'une entry, le système génère automatiquement une variable `entry` directement accessible dans notre template. Grâce à cette variable et au système de routing de Craft vous n'avez besoin de rien d'autre pour afficher vos contenus sur une page de détail.

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

##### Page de categories et variable "category"

Le même principe est d'application lorsqu'une page de catégorie est affichée. Lorsqu'une URL défine comme une URL de catégorie est affichée par le système, Craft défini automatiquement une variable `category` que vous pouvez utiliser directement au sein de vos templates.

```twig
{#
 # This template gets loaded whenever a Category URL is
 # requested. That’s because a Category group Template setting is
 # set to “news/index”, the path to this template.
 #}

{# layout used #}
{% extends "layouts/_base.html" %}

{% set allCategories = craft.categories.group('newsTopics').find() %}

{#
 #	- craft automatically creates a 'category' variable if it detects you are on a category template
 #  - we are just checking whether that category variable exists or not
 #  - depending on its existence, we set our list of entries
 #}

{% if category is defined %}
	{% set currentCategory = category.slug %}
	{% set entries = craft.entries.section('news').relatedTo(category).limit(10).find(); %}
{% else %}
	{% set currentCategory = 'all' %}
	{% set entries = craft.entries.section('news').limit(10).find(); %}
{% endif %}

{% block content %}

	{# display entries list #}
	{% for entry in entries %}
		{% if loop.first %}<ul>{% endif %}
			<article>
				<p class="meta-info"><time datetime="{{ entry.postDate|date("Y-m-d") }}">{{ entry.postDate|date("F j, Y") }}</time></p>
				<h2><a href="{{ entry.url }}">{{ entry.title }}</a></h2>
				<p>{{ entry.summary }}</p>
			</article>
		{% if loop.last %}</ul>{% endif %}
	{% endfor %}

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

Les globals stockent du contenu qui va, comme leur nom l'indique, être disponible globalement pour tous les templates.

Vous pouvez y accéder très facilement via leur handle de global set suivi de leur handle de global. Par exemple, pour une global appelée `tagline` dans un global set `companyInfo`:

`{{ companyInfo.tagline }}`

#### Tags

Dans Craft, on accède aux tags avec `craft.tags`, qui [possède un certain nombre de paramètres](http://buildwithcraft.com/docs/templating/craft.tags) et fonctionne dans l'ensemble comme `craft.entries` dans la mesure où il retourne un objet [ElementCriteriaModel](http://buildwithcraft.com/docs/templating/elementcriteriamodel).

Deux articles sur buildwithcraft.com vous montrent comment obtenir une [liste de tous les tags utilisés par les entries d'une section](http://buildwithcraft.com/help/active-tags), ou encore comment créer, à l'aide d'une route dynamique, [une page d'archive reprenant toutes les entries liées à un tag](http://buildwithcraft.com/help/tag-urls).

#### Users

Le tag `craft.users` permet d'accéder aux utilisateurs de votre site. Ce tag possède lui aussi [un certain nombre de paramètres, dont certains lui sont propres](http://buildwithcraft.com/docs/templating/craft.users). Son fonctionnement est semblable au tag `craft.entries` dans la mesure où il retourne un objet [ElementCriteriaModel](http://buildwithcraft.com/docs/templating/elementcriteriamodel).

#### Assets et transformations

Le tag `craft.assets` permet d'accéder aux Assets de votre site. Ce tag possède lui aussi [un certain nombre de paramètres, dont certains lui sont propres](http://buildwithcraft.com/docs/templating/craft.assets). Son fonctionnement est semblable au tag `craft.entries` dans la mesure où il retourne un objet [ElementCriteriaModel](http://buildwithcraft.com/docs/templating/elementcriteriamodel).

Si vos assets sont des images, Craft vous permet d'effectuer des transformations de celles-ci, soit à l'upload (via votre control panel: Settings > Assets > Image Transforms), soit dynamiquement dans vos templates.

Si vous définissez une transformation directement dans le control panel et que vous lui donnez comme handle `thumbnail` vous pouvez y accéder dans vos templates de la façon suivante :

```twig
{% if myAssetField | length %}
	{% set heroImage = myAssetField.first() %}
	<img src="{{ heroImage.getUrl('thumbnail') }} width="{{ asset.getWidth('thumbnail') }}" height="{{ asset.getHeight('thumbnail') }}" alt="{{ heroImage.title }}">
{% endif %}
```

Vous pouvez également définir dynamiquement une transformation dans vos templates

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

## Aller plus loin

### Configurations de Redactor

Craft utilise [Redactor](http://imperavi.com/redactor/) comme WYSIWYG pour les champs de type "rich text". Lorsque vous créez un champ de ce type, vous pouvez choisir d'appliquer une configuration précise de Redactor pour ces champs.

Ces configurations peuvent être facilement créées et modifiées à l'aide de simples fichiers JSON stockés dans le répertoire `craft/config/redactor`. Le nom donné à la configuration est simplement celui de votre fichier JSON.

### Configurations pour environnements multiples

Craft fournit nativement une façon simple de gérer des environnements multiples (local, dev, online) via [l'utilisation d'Arrays imbriqués](http://buildwithcraft.com/docs/multi-environment-configs) dans les fichiers `general.php` et `db.php` inclus dans le dossier `config/`.

Les valeurs dans le tableau `*` sont appliquées à tous les environnements

**Exemple**: le fichier craft/config/general.php

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

Pour compléter cela, vous pouvez également utiliser ce que Craft appelle des [variables d'environnement](http://buildwithcraft.com/docs/multi-environment-configs#environment-specific-variables). Ces variables vont pouvoir être utilisées pour créer des configurations dynamiques dans votre control panel.

```
return array(
    '*' => array(
        'omitScriptNameInUrls' => true,
    ),

    'domain.dev' => array(
        'devMode' => true,

        'environmentVariables' => array(
            'siteUrl'        => 'http://www.domain.dev/',
            'fileSystemPath' => '/users/sitename/htdocs/'
            'cpTrigger'      => 'adminpanel',
        )
    ),

    'domain.com' => array(
        'cooldownDuration' => 0,

        'environmentVariables' => array(
            'siteUrl'        => 'http://www.domain.com/',
            'fileSystemPath' => '/users/domain/htdocs/'
            'cpTrigger'      => 'adminpanel',
        )
    )
);
```

Ces différents environnements peuvent également être utilisés pour les paramètres de configuration de votre base de données dans le fichier `craft/config/db.php`.

**Exemple**: le fichier craft/config/db.php

```
return array(
    '*' => array(
        'tablePrefix' => 'craft',
    ),

    'domain.dev' => array(
        'server' => 'localhost',
        'user' => 'root',
        'password' => 'password',
        'database' => 'domain_craft',
    ),

    'domain.com' => array(
        'server' => 'localhost',
        'user' => 'user',
        'password' => 'strongpassword',
        'database' => 'domain_craft',
    ),
);
```

### Matrix

[Matrix](http://buildwithcraft.com/features/matrix) est un type de champ particulièrement [intéressant et puissant](http://buildwithcraft.com/features/matrix) dont vous disposez dans Craft. Il vous permet de créer des champs qui combinent différents blocs et de définir la structure pour chacun de ces blocs.

Vous pourriez par exemple créer un champ `modularBody` avec la configuration suivante:

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

Une telle configuration permettra à vos utilisateurs de composer leurs items comme ils le souhaitent en créant et en arrangeant à leur guise n'importe quelle combinaison de blocs textes, de block quotes et de blocs images.

Au niveau du templating, vous pouvez également contrôler très précisément le code HTML généré. Nous utilisons ici [le tag `switch` propre à Craft](http://buildwithcraft.com/docs/templating/tags#switch).

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

## Exercices

### A faire ensemble

Construire un blog simple. Chaque post devra inclure une image afin de pouvoir pratiquer les transformations d'images dans Craft.

### A faire seul

1. Ajouter une page d'archive à votre blog qui permet de visualiser les posts par année en utilisant du dynamic routing.
2. Créer une section "portfolio" vous permettant de présenter vos travaux. Créer une navigation dans les pages de détail permettant de naviguer de page de détail à page de détail.

## Ressources

- [Documentation officielle](http://buildwithcraft.com/docs/introduction) de Craft
- [Articles Help & Support](http://buildwithcraft.com/help) officiels
- [Documentation Twig](http://twig.sensiolabs.org/doc/templates.html) pour les template designers
- [Screencast Mijingo](https://mijingo.com/products/screencasts/craft-cms-tutorial/) : une bonne entrée en matière.
- [Straight up Craft](http://straightupcraft.com/): Un site de ressources concernant Craft et une liste des développeurs actifs et des plugins existants.
- [Quelques videos](http://straightupcraft.com/learn-craft-cms) sur Straight up Craft
- [On the Rocks](https://github.com/pixelandtonic/ontherocks) : un site de démonstration dont le code est sur Github : pratique pour voir comment sont faits les templates.
- [Craft sur Google Plus](https://plus.google.com/communities/106505340287442511226) : la communauté Craft sur G+
- [Episode de CTRL+CLICK CAST](http://ctrlclickcast.com/episodes/crafty-sites-with-brandon-kelly) avec Brandon Kelly
- [Interview de Brandon Kelly](http://www.thenerdary.net/post/48123188844/interview-with-brandon-kelly-creator-of-craft) par the Nerdary
- [Introduction au templating avec Craft](http://withchief.com/blog/basics-of-templating-in-craft) par [Jamie Pittock](https://twitter.com/jamiepittock) sur Withchief
- [Craft Your Content With Markdown And Matrix](http://experiencehq.net/blog/craft-with-markdown-and-matrix) par l'inimitable [Stephen Lewis](https://twitter.com/monooso) : quelques bons exemples au niveau du templating.
- [Craft Cookbook](http://www.craftcookbook.net) : sites d'exemples courts et précis. Bonne introduction au templating avec Craft et Twig.
- [Making Sense of Twig](http://www.slideshare.net/brandonkelly212/twig-for-designers): une présentation de [Brandon Kelly](https://twitter.com/brandonkelly) et une très bonne introduction à Twig.
- [Real World Craft Tips & Tricks](https://speakerdeck.com/trevor_davis/real-world-craft-tips-and-tricks): une présentation de [Trevor Davis](https://twitter.com/trevor_davis). Quelques bons trucs et astuces si vous ne connaissez pas encore Twig et Craft.
- [Craft stackexchange site](http://craftcms.stackexchange.com/): poser vos questions concernant Craft et obtenir des réponses rapidement
