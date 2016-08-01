# Craft by Pixel&Tonic: Introduction

## Introduction

### Technologies et ethos

[Craft](http://buildwithcraft.com/) est un CMS récent créé par [Pixel & Tonic](http://pixelandtonic.com/). Craft est développé à l'aide de technologies open source comme PHP et MySQL et s'appuie sur un framework PHP qui a fait ses preuves: [Yii](http://www.yiiframework.com/).

Comparé à d'autres produits, Craft est axé sur l'essentiel: définir et gérer vos contenus, et ce de la façon la plus modulaire et flexible possible. Si vous avez besoin d'un moteur de commentaires, d'un forum ou encore d'e-commerce, il faudra vous tourner vers des plugins.

### Un modèle de pricing modulaire

Cette flexibilité est présente jusque dans [le pricing](http://buildwithcraft.com/pricing) du produit lui même.

- La version "Core" de Craft est gratuite et vous permet de profiter de toutes les fonctionnalités de Craft hormis la gestion utilisateurs, les fonctionnalités multilingues et celles liées à des stockages de données externe (Amazon S3, Rackspace Cloud et Google Cloud Storage).
- Une version "Client" à 199$ vous permet de disposer en plus d'une gestion utilisateurs basique et de fonctionnaités de personnalisation au niveau des écrans de login ou des messages utilisateurs.
- Une version "Pro" à 299$ vous permet de disposer de toutes les fonctionnalités de Craft, y compris ses fonctionnalités multilingue et de gestion d'utilisateurs.

### Une plateforme de e-commerce intégrée

En Décembre 2015, Pixel & Tonic a sorti [Craft Commerce](https://craftcommerce.com), une plateforme de e-commerce pour Craft CMS. Une licence coûte $700 et vous donne droit à [une liste impressionnante de fonctionalités](https://craftcommerce.com/features), des mises à jour gratuites pendant toute la durée de votre projet ainsi qu'un support de qualité.

Nous ne couvrirons pas Craft Commerce lors de cette introduction à Craft CMS. Sachez seulement que Craft Commerce fait largement appel aux mêmes concepts que Craft, que ce soit pour la création de votre data structure ou au niveau de vos templates. Vous vous sentirez en tarrain familier. Si vous cherchez un module de e-commerce étroitement intégré avec un CMS, Craft Commerce est certainement une option à considérer.

### Atouts

Les principaux atouts de Craft sont à mon sens:

- Une flexibilité dans la définition de votre data structure rarement égalée. Les 16 field types disponibles par défaut permettent une approche extrêmement modulaire.
- A propos de champs et de modularité, Matrix est véritablement un atout de Craft en ce qui concerne la flexibilité de vore data structure tout en préservant votre contrôle sur le code front-end généré.
- [L'utilisation de Twig comme language de templating](http://twig.sensiolabs.org/doc/templates.html): cela induit un temps d'apprentissage mais les gains en termes de puissance, de modularité et de flexibilités sont étonnants. Toute la puissance et la maturité de Twig sont au services de vos templates.
- Une quantité impressionnantes de fonctionnalités qui rendront la vie de vos clients plus facile: live preview, control panel responsive, one click updates, etc.
- Un découplage important entre votre structure de dossiers et de fichiers et la manière dont sont construites les URL. La structure de vos URL est donc extrêmement flexible.
- Une solution intégrée et complète pour les sites multilingues.
- Une équipe de développement et de support fantastique

## Installation et configuration

### Installation

Après avoir vérifié que votre serveur est capable de faire tourner Craft, il ne vous reste qu'à suivre une [procédure d'installation simple](https://craftcms.com/docs/installing) et à vérifier l'ensemble de vos permissions de dossiers et de fichiers. Cela devrait être assez rapide.

### Version control

L plupart des développeurs utilisent Git pour maintenir leur code et en gérer les différentes versions. Certains préfèrent ne pas include Craft dans le repository de leurs projets. Pour ma part, je préfère inclure Craft. Cela rend les choses plus facile lorsqu'on travaille en équipe: une fois Craft mis à jour, il l'est pour l'ensemble de l'équipe. Il est cependant préférable [d'include certains dossiers dans son fichier .gitignore](https://craftcms.com/support/craft-storage-gitignore) pour éviter les problèmes. Voici ce que j'utilise généralement:

```
# Craft stuff
# ----------------------------
craft/storage/backups/
craft/storage/runtime/

# User uploaded files
# ---------------------------
public/uploads/
```

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
    'siteUrl' => 'http://www.domain.dev/',

    'environmentVariables' => array(
      'basePath'   => '/localprojects/sitename/htdocs/'
      'baseUrl'     => 'http://www.domain.dev/'
      'cpTrigger'  => 'adminpanel'
    )
  ),

  'domain.com' => array(
    'cooldownDuration' => 0,
    'siteUrl' => 'http://www.domain.com/',

    'environmentVariables' => array(
      'basePath'   => '/var/www/sitename/htdocs/'
      'baseUrl'    => 'http://www.domain.com/'
      'cpTrigger'  => 'adminpanel',
    )
  )
);
```

Vous pouvez maintenant utiliser ces variables d'environnement dans votre Control panel, par exemple pour définir les chemins et URL de vos asset sources pour les rendre dynamiques en fonction de l'environnement.

```
{basePath}assets/images/
{baseUrl}assets/images/
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

### Configurations de Redactor

Craft utilise [Redactor](http://imperavi.com/redactor/) comme WYSIWYG pour les champs de type "rich text". Lorsque vous créez un champ de ce type, vous pouvez choisir d'appliquer une configuration précise de Redactor pour ces champs.

Ces configurations peuvent être facilement créées et modifiées à l'aide de simples fichiers JSON stockés dans le répertoire `craft/config/redactor`. Le nom donné à la configuration est simplement celui de votre fichier JSON.

## Définir et structurer votre projet: vos outils

Craft vous permet de structurer les données de votre site de façon extrêmement flexible et modulaire.

### Sections, Entries et entry types

Avec Craft, vos contenus vont principalement "vivre" dans des entries, elles-même contenues dans des sections.

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

A côté des sections et des entries, les [globals](http://buildwithcraft.com/docs/globals) peuvent être utilisées pour stocker du contenu auquel il est possible d'accéder dans vos templates: tagline, coordonnées de contact, code Google Analytics, etc.

Vous pouvez créer des groupes de contenus en utilisant les global sets. Chaque set de globals possède son field layout et donc sa propre data structure. Les différences notables avec les sections et les entries sont que les globals ne possèdent pas d'URL et ne peuvent donc pas être visualisées comme les entries à l'aide de la fonction "live preview".

### Users

Dans Craft, les [utilisateurs](http://buildwithcraft.com/docs/users) servent à gérer les permissions données aux divers utilisateurs du système.

Les utilisateurs peuvent être assignés à divers groupes. Ces groupes sont utilisés pour gérer les permissions données aux utilisateurs qui en sont membres. Un utilisateur peut être assigné à plusieurs groupes.

La data structure des utilisateurs peut être aussi complexe que souhaitée. Via un unique Field layout, les champs souhaités peuvent être assignés à la data structure de tous vos utilisateurs.

### Assets

Les [assets](http://buildwithcraft.com/docs/assets) vous permettent de gérer vos fichiers Craft. Vos Assets sont enregistrés dans des Asset Sources qui correspondent à des dossiers physiques sur votre serveur.

De multiples [transformations d'images](http://buildwithcraft.com/docs/image-transforms) peuvent être réalisées sur vos assets: crop, scale, quality, etc. Ces transformations vous permettent de créer vos thumbnails pour l'ensemble de votre site à partir d'une image de base.

Un field layout est associé à chaque Asset Source et vous permet de créer une data-structure en y d'associant des custom fields. Vos documents peuvent donc avoir une data-structure différente de vos photos par exemple.

### Tags

Les [tags](http://buildwithcraft.com/docs/tags) vous permettent de créer des *folksonomies* et de les appliquer à vos Entries, Users ou Assets.

Chaque tag doit être assigné à un groupe et chaque groupe de tags possède un field layout. Vous pouvez donc créer des structures de données assez complexes pour chacun de vos groupes de tags.

### Categories

Les [categories](http://buildwithcraft.com/docs/categories) vous permettent de créer des taxonomies et de les appliquer à vos Entries, Users ou Assets.

Chaque catégorie doit être assignée à un groupe.

- Chaque groupe possède un field layout.
- Au sein de chaque groupe vous pouvez spécifier la structure des URLs pour vos catégories et vos sous-categories
- Pour chaque groupe de categories, vous pouvez spécifier le template qui va être chargé quand une URL de catégorie est demandée.

### Relations

L'une des grandes forces de Craft c'est qu'il est possible très facilement de créer des [relations](http://buildwithcraft.com/docs/relations) entre Entries, Users, Assets et Tags.

Pour cela, Craft met à votre disposition des champs relationnels spécifiques:

- **Assets**: permet d'établir une relation "one to one" ou "one to many" vers des Assets
- **Entries**: permet d'établir une relation "one to one" ou "one to many" vers des Entries
- **Users**: permet d'établir une relation "one to one" ou "one to many" vers des Users
- **Tags**: permet d'établir une relation "one to one" ou "one to many" vers des Tags
- **Categories**: permet d'établir une relation "one to one" ou "one to many" vers des Catégories

Pour chacun des ces tags, vous pouvez spécifier combien d'items peuvent être liés et de quelles sources ils proviennent.

Pour exploiter ces relations dans vos templates, Craft met à votre disposition un outil extrêmement puissant: le paramètre [`relatedTo`](http://buildwithcraft.com/docs/relations#the-relatedTo-param), utilisable avec `craft.entries`, `craft.users`, `craft.assets`, `craft.tags` et `craft.categories`.

### Routing

L'un des autres éléments intéressant de Craft c'est le [routing](http://buildwithcraft.com/docs/routing) dynamique, qui permet de séparer structure des URLs et architecture de dossiers et de fichiers.

Comme nous l'avons déjà vu, Craft cous permet de spécifier une structure d'URL propre pour chaque Entry, User, Asset, Tags and Categories.

Si cela ne suffit pas à couvrir tous vos besoins, vous pouvez égalment créer des routes dynamiques. Pour chaque route créée, vous pouvez spécifier quel template doit être chargé par Craft.

Il est possible créer des routes et de spécifier quel template doit être chargé par Craft. Un exemple facile à comprendre est [un template donnant accès à une archive des entries classées par année](http://buildwithcraft.com/help/entry-archive#yearly-archive-pages).

En créant une route dynamique `blog/archive/{year}` renseignant le template `blog/archive`, les URL `blog/archive/2013` et `blog/archive/2012` vont charger le même template et rendre disponible une variable `year` utilisable par Twig et par Craft avec les paramètres [`after`](http://buildwithcraft.com/docs/templating/craft.entries#after) et [`before`](http://buildwithcraft.com/docs/templating/craft.entries#before).

Si vous avez besoin de plus de possibilités que celles offertes par le control panel, sachez que [les routes peuvent également être gérées via un fichier](http://buildwithcraft.com/docs/routing#advanced-routing) `config/routes.php`, ce qui vous donne accès à du matching d'URL en utilisant des expressions régulières.

### Recherche

Craft possède également un [système de recherche très puissant](http://buildwithcraft.com/docs/searching) basé sur un paramètre `search` utilisable avec les tags `craft.entries`, `craft.users`, `craft.assets` et `craft.tags`.

Pour des questions de performance, Craft fonctionne avec des Index pour ses fonctions de recherche. Il est possible d'actualiser ces Index directement depuis le Control Panel.

La construction de [formulaires de recherche dynamiques pour le front-end](http://buildwithcraft.com/docs/templating/search-form) de votre projet est également très simple. Il suffit pour cela de faire appel au tag `[craft.request]` pour [récupérer un paramètre passé en GET/POST](http://buildwithcraft.com/docs/templating/craft.request) par votre formulaire.

## Templating 1: utiliser Twig

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

Convertir une string en title case:

```twig
{{ entry.title|title }}
```

Réaliser des opérations sur les dates et les formatter:

```twig
{{ entry.postDate|date_modify("+1 day")|date("m/d/Y") }}
```

Déterminer la longueur d'une string, d'un array ou d'un objet:

```twig
{% set allEntries = craft.entries.section('blog').limit(null).order('postDate desc').find() %}
{{ allEntries|length }}
```

Vous pouvez également appliquer des filtres à plusieurs lignes de votre template et pas seulement à une variable:

```twig
{% filter upper %}
  This text becomes uppercase
{% endfilter %}
```

Ces filtres peuvent également être combinés:

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

Twig vous permet de créer des structures de contrôles à l'aide de conditionnels.

```twig
{% if allEntries|length %}
  <p>There is at least one entry here</p>
{% endif %}
```

**If / else**

```twig
{% if userGender == "female" %}
  <p>Hello, how are you doing?<p>
{% else %}
  <p>Hey dude, how are things?</p>
{% endif %}
```

**Conditions imbriquées**

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

**Conditions cumulées**

```twig
{% if user and user.male %}
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

  <title>{% if htmlTitle is defined %}{{ htmlTitle }}{% else %}My generic title{% endif %} - Mysite</title>

  <meta name="viewport" content="width=device-width, initial-scale=1">

  <link rel="shortcut icon" href="{{ siteUrl }}favicon.ico">
  <link rel="stylesheet" media="screen" href="{{ siteUrl }}assets/css/screen.css">
  <link rel="stylesheet" media="print" href="{{ siteUrl }}assets/css/print.css">

  <script src="{{ siteUrl }}assets/js/libs/modernizr.js"></script>

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

```twig
{% macro dateText(date) %}
  {{ date|date('F j, Y') }}
{% endmacro %}

{% macro dateNumeric(date) %}
  {{ date|date('d.m.Y') }}
{% endmacro %}
```

Les macros sont appelées / importées à l'aide du tag `{% import %}`

Si la macro est définie dans le même fichier

```twig
{% import _self as formErrors %}{{ formErrors.errors(entry.allErrors) }}```

Si la macro est définie dans un fichier extérieur

```twig
{% import "_macros/dates" as dateHelpers %}{{ dateHelpers.dateText(entry.postDate) }}```
## Templating 2: Récupérer vos données avec Craft

Avec Craft, vous intéragissez avec la base de données en utilisant des objets ElementCriteriaModel. Cela à l'air très compliqué mais c'est en fait un concept assez simple:

1. vous créez un objet ElementCriteriaModel pour le type de données que vous souhaitez récupérer dans la base de données (entries, users, assets, etc.)
2. vous spécifiez les paramètres à utiliser (limit, order, filters, etc.)
3. Craft va chercher les données correspondant à vos critère dans la base de données
4. Craft retourne un objet ElementModel ou un array d'objets ElementModel ([EntryModel](http://buildwithcraft.com/docs/templating/entrymodel) for entries, [UserModel](http://buildwithcraft.com/docs/templating/usermodel) for users, [AssetFileModel](http://buildwithcraft.com/docs/templating/assetfilemodel) for assets, [CategoryModel](http://buildwithcraft.com/docs/templating/categorymodel) for categories and [TagModel](http://buildwithcraft.com/docs/templating/tagmodel) for tags).
5. Vous pouvez ensuite afficher ces objets ou arry d'objets dans vos templates.

`craft.entries`, `craft.users`, `craft.assets`, `craft.categories` et `craft.tags` seront vos principaux outils de travail. Nous nous centrerons ici principalement sur `craft.entries`. Les autres tags ayant un fonctionnement très similaire, il vous sera facile d'appliquer les mêmes principes.

### Entries

[`craft.entries`](http://buildwithcraft.com/docs/templating/craft.entries) est le tag que vous allez utiliser pour récupérer vos entries.

- `craft.entries.find()` vous permet de récupérer toutes les entries qui correspondent à vos critères
- `craft.entries.total()` vous permet de récupérer le nombre total des entries correspondant à vos critères
- `craft.entries.first()`, `craft.entries.last()` et `craft.entries.nth(n)` vous permettent de récupérer la première, la dernière ou la nième des entries correspondant à vos critères
- `craft.entries.ids()` vous permet de récupérer la liste des ids des entries correspondant à vos critères

#### Deux syntaxes différentes

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

#### Pas de résultats

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

#### "Loop", "cycle" et "is divisible by"

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

#### Pagination

Craft vous permet de [paginer vos résultats](http://buildwithcraft.com/docs/templating/tags#paginate) à l'aide du tag  `{% paginate %}` et de construire une interface de pagination à l'aide des variables qui l'accompagnent. Attention, le tag `{% paginate %}` nécessite un objet ElementCriteriaModel comme paramètre. N'utilisez simplement pas la méthode `find()` de l'objet.

```twig
{% paginate craft.entries.section('news').limit(5) as paginate, entries %}

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

#### Page de détail et variable "entry"

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

#### Page de categories et variable "category"

Le même principe est d'application lorsqu'une page de catégorie est affichée. Lorsqu'une URL défine comme une URL de catégorie est affichée par le système, Craft défini automatiquement une variable `category` que vous pouvez utiliser directement au sein de vos templates.

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
   # - craft automatically creates a 'category' variable if it detects you are on a category template
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

Les globals stockent du contenu qui va, comme leur nom l'indique, être disponible globalement pour tous les templates.

Vous pouvez y accéder très facilement via leur handle de global set suivi de leur handle de global. Par exemple, pour une global appelée `tagline` dans un global set `companyInfo`:

`{{ companyInfo.tagline }}`

### Tags

Dans Craft, on accède aux tags avec `craft.tags`, qui [possède un certain nombre de paramètres](http://buildwithcraft.com/docs/templating/craft.tags) et fonctionne dans l'ensemble comme `craft.entries` mais retourne un objet ou un array d'objets [`TagModel`](http://buildwithcraft.com/docs/templating/tagmode).

Deux articles sur buildwithcraft.com vous montrent comment obtenir une [liste de tous les tags utilisés par les entries d'une section](http://buildwithcraft.com/help/active-tags), ou encore comment créer, à l'aide d'une route dynamique, [une page d'archive reprenant toutes les entries liées à un tag](http://buildwithcraft.com/help/tag-urls).

### Users

Le tag `craft.users` permet d'accéder aux utilisateurs de votre site. Ce tag possède lui aussi [un certain nombre de paramètres, dont certains lui sont propres](http://buildwithcraft.com/docs/templating/craft.users). Son fonctionnement est semblable au tag `craft.entries` mais il retourne un objet ou un array d'objets [`UserModel`](http://buildwithcraft.com/docs/templating/usermodel).

### Assets et transformations

Le tag `craft.assets` permet d'accéder aux Assets de votre site. Ce tag possède lui aussi [un certain nombre de paramètres, dont certains lui sont propres](http://buildwithcraft.com/docs/templating/craft.assets). Son fonctionnement est semblable au tag `craft.entries` dans la mesure où il retourne un objet [`AssetFileModel`](http://buildwithcraft.com/docs/templating/assetfilemodel).

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

Une telle configuration permettra à vos utilisateurs de composer leurs items comme ils le souhaitent en créant et en arrangeant à leur guise n'importe quelle combinaison de modules textes, quotes et images.

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

Personellement, je préfère simplifier mes templates et placer l'ensemble des vues pour mes Matrix Blocks dans des fichiers dédiés indépendants et auto-suffisants. Ces fichiers sont auto-suffisants dans la mesure où ils contiennent l'ensemble des variables et autres objets utilisés pour les transformations d'images. Cela permet également de réutiliser ces fichiers ailleurs si besoin est en ayant une source unique.

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

## Pour aller plus loin

Voici quelques techniques et concepts à explorer pour utiliser Craft de façon un peu plus avancée.

### Créer des queries complexes en utilisant Twig pour manipuler les `ElementCriteriaModels`

Comme nous l'avons vu plus haut, les prinicpaux tags de Craft comme par exemple `craft.entries` acceptent des objets comme paramètres. Twig permet facilement de créer et de manipuler des objets à l'aide de filtres tels que `merge` et `slide`. Craft possède également des filtres propres tels que `without` et `intersect` qui s'avèrent bien utiles.

En combinant ces deux éléments, il devient possible de créer des [queries complexes](https://webstoemp.com/blog/manipulating-craft-elementcriteriamodel-with-twig/) et de construire des [fonctionnalités relativement avancées](https://webstoemp.com/blog/combined-searches-and-filters-craft-cms/) assez facilement.

Voici un exemple simple. Vous avez laissé la possiblité à vos utilisateurs de choisir 3 blogposts à afficher sur la homepage. Vous avez donc créé un champs entries dont vous avez spécifié la limite à 3, puisque votre design de la homepage possède seulement 3 emplacements. Vous souhaitez que ces trois emplacements soient toujours remplis. Si l'utilisateur a choisi 1, 2 ou 3 blogposts à l'aide du champs entries, vous voulez afficher ces blogposts d'abord et complèter éventuellement les emplacements restants avec les blogposts les plus récents. Aucun blogost ne peut êre affiché deux fois. Voici comment faire:

```twig
{#
 # 1. Create array containing Ids of chosen blogpsts
 # 2. If not up to 3 items, get the Ids of the 3 most recent blogposts and remove chosen blogposts Ids from that list
 # 3. Get your final ElementCriteria model using your list of Ids
##}

{% set blogpostsIds = entry.homeProjects.ids() %}

{% if blogpostsIds | length < 3 %}
  {% set recentBlogpostsIds = craft.entries.section('blogposts').limit(3).ids() | without(blogpostsIds) %}
  {% set blogpostsIds = blogpostsIds | merge(recentBlogpostsIds) | slice(0,3) %}
{% endif %}

{% set blogposts = craft.entries.section('blogposts').id(blogpostsIds).fixedOrder(true).find() %}

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

### Interfaces de navigation en utilisant un channel e type structure

Si vous essayez d'avoir une entry (single) pour chaque page de votre site, Craft permet facilement de disposer d'une navigation principale flexible et pouvant même être mise à jour par vos utilisateurs. J'utilise généralement une section de type structure pour fournir ce genre de fonctionnalité. Voici un exemple  avec une navigation à un seul niveau. Il suffit de créer une section de type structure `mainnav` et deux champs: `mainnavLabel` (textfield) et `mainnavLink` (entries, limité à 1 entry et à des entries de type single). Une simple boucle `for` suffit à créer notre navigation.

```twig
{% set nav = craft.entries.section('mainnav').find() %}
{% for item in nav %}
  {% if loop.first %}<ul clas="c-mainnav">{% endif %}

    {% set navSection = item.mainnavLink.first() %}
    {% set navCurrentClass = (entry is defined and navSection.uri == entry.uri) ? "c-mainnav__link--current" : "" %}

    {% if single %}
      <li class="c-mainnav__item">
        <a class="c-mainnav__link  {{ navCurrentClass }}" href="{{ navSection.url }}">{{ item.mainnavLabel }}</a>
      </li>
    {% endif %}

  {% if loop.last %}</ul>{% endif %}
{% endfor %}
```

C'est évidemment un cas assez simple mais cette méthode peut être utilisée pour bon nombre d'interfaces de navigation. Cette technique commence à poser problème lorsque vous avez une navigation de plus de deux niveaux.

### Sites multilingues

Les sites multilingues sont en général assez complexes. Craft rend les choses plus faciles dans la mesure où le multilingue est géré nativement. Les bases sont assez simples et sont détaillée dans l'article d'aide suivant: "[Setting Up a Localized Site](https://craftcms.com/docs/localization-guide)".

Je vis et je travaille en Belgique, un pays qui compte trois langues officielles. les capacités de Craft à gérer les sites multilingues sont une bouffée d'air frais. Par la force des choses, j'ai travaillé sur un certain nombre de sites multilingues. Voici [un blogpost détaillant les différentes techniques et macros](https://webstoemp.com/blog/craft-multilingual-websites-tips/) que j'utilise le plus souvent pour ce genre de projets. J'espère que cela vous facilitera la vie.

### Optimisation de queries et eager-loading

Un problème courant avec les bases de données est connu sous le nom de "problème n+1". Ce problème se pose lorsque vous devez traverser ue collection d'objets ayant des relations avec d'autres: pour chaque objet dans la collection, `1 + n` queries sont générées puisque chaque objet de la collection peut être lié à `n` objets. Une exmeple simple avec Craft consite à demander une série d'entries, chaque entry ayant un asset lié. Lorsque Craft charge ces entries, il va créer `n` queries additionnelles pour vérifier si un asset lié existe ou pas. C'est le comportement par défaut de Craft, qui porte le nom de "lazy loading".

"[Eager-loading](https://craftcms.com/docs/templating/eager-loading-elements)" est une façon d'indiquer à Craft que lorsqu'il va chercher ces entries, chacune d'entre elles possède également un asset qu'il faudra aller chercher lui aussi. Une fois prévenu, Craft va aller chercher les entries et les assets liés en utilisant le moins de queries MySQL possible. Pour faire de l'eager loading, il suffit d'utiliser le paramètre `with` dans vos tags `craft.entries` par exemple.

Lazy loading (assets):

```twig
{% set items = craft.entries({
    section: 'blogposts',
}) %}

{% for item in items %}
  {# display item #}
{% endfor %}
```

Eager loading (assets):

```twig
{% set items = craft.entries({
    section: 'blogposts',
    with: [
      'blogpostImage'
    ]
}) %}

{% for item in items %}
  {# display item #}
{% endfor %}
```

Utilsier de l'eager loading peut devenir plus complexe. Vous pouvez utiliser l'eager loading avec entries, assets, catégories, tags ou users. Vous pouvez également l'utiliser avec des assets transforms et des Matrix Blocks. Vous pouvez également utiliser l'eager loading pour charger des éléments liés imbriqués, ce qui m'a déjà coûté quelques cheveux blancs. [Un excellent guide concernant l'eager loading](https://straightupcraft.com/articles/examples-of-eager-loading-elements-in-twig-and-php) est disponible sur Straight Up Craft si vous voulez vous pencher d'avantage sur la question.

Voici néanmoins un exemple plus complexe à utiliser si chaque entry possède un champs Matrix qui contient un champs asset pour lequel un tranform nommé `thumbnail` est défini dans le template et appliqué:

```twig
{% set thumbnail = {
	mode: 'crop',
  position: 'center-center'
	width: 800,
	height: 450,
	quality: 75
}%}

{% set items = craft.entries({
    section: 'blogposts',
    with: [
      ['matrixFieldHandle.blockTypeHandle:assetFieldHandle', {
        withTransforms: [thumbnail]
      }]
    ]
}) %}

{% for item in items %}
  {# display item #}
{% endfor %}
```

### Le tag `{% cache %}`

[Le tag `{% cache %}`](https://craftcms.com/docs/templating/cache) peut être utilisé pour améliorer la performance de certaines parties de templates. Lorsque le template est chargé pour la première fois, les parties de templates cachées vont exécutées les queries nécessaires pour récupérer les élements souhaités et vont ensuite stocker l'HTML produit dans la base de données. Lorsque ce template est chargé après cette requète initiale, Craft va seulement chargé le HTML stocké dans la base de données au lieu d'exécuter à nouveau l'ensemble des queries.  

Craft va automatiquement effacer les caches lorsque des élements compris entre les tags `{% cache %}` et `{% endcache %}` sont supprimés ou mis à jour. Vous pouvez également spécifier la durée de vie de vos caches. La durée par défault est celle spécifiée par le paramètre de configuration `[cacheDuration](https://craftcms.com/docs/config-settings#cacheDuration)` (sa valeur par défaut est d'un jour).

```twig
{% cache for 1 month %}
{% endcache %}

{% cache for 3 days %}
{% endcache %}

{% cache for 7 hours %}
{% endcache %}
```

L'utilisation de caching doit idéalement se faire sur des templates déjà optimisés. Une couche de caching appliquée sur un template qui n'est pas optimisé est un emplatre sur une jambe de bois.

Bien utilisé, le caching vous fera gagner pas mal de performance. Le tag `{% cache %}` est principalement utilisé lors des cas de figures suivants:

- Longues listes d'entries
- Des champs Matrix comportant des champs relationnels (entries, assets, users, categories, tags)
- Affichage de données provenant d'un site tiers (Twitter, etc.)

Vous pouvez tester l'efficacité de vos tratégies de caching en activant le `devMode`, en supprimant les caches dans le control panel (paramètres, Outils) et en rafraîchissant votre page en onsulatant la Console dans les outils de dévelopement de votre navigateur. Regardez le "profiling summary report", qui vous donnera le nombre total de queries exécutées par Craft et le temps de rendu de la page.

Lors du premier refresh de la page après avoir vidé les caches, toutes les queries seront exécutées et le HTML généré sera enregistré dans la base de données. Lorsque vous rafraichirez la page our la seconde fois, Craft ne fera que récuérer le HTML et vous devriez constater une baisse importante du nombre de queries et du temps de rendu de la page.

### Formulaires front-end: entry form et guest entries

Par défaut, Craft permet la création d'[entry forms](https://craftcms.com/docs/templating/entry-form) pour le front-end de votre site. Ces formulaires peuvent uniquement petre utilisés par des utilisateurs enregistrés. Vous pouvez également autoriser des utilisateurs anonymes à poster des entries en utilisant le plugin [Guest Entries](https://github.com/pixelandtonic/GuestEntries) développé par Pixel&Tonic. Ce plugin vous permt de choisir pour quelles sections vous souhaitez autoriser des entries anonymes et quel auteur par défaut doit petre spécifié pour ces entries.

En couplant cela avec un plugin de notification tel que Sprout Email ou un plugin que vous aurez écrit en utilisant les événements et le service email de Craft, vous pouvez facilement créer des applications simples comme un système de booking pour un événement gratuit.

La syntaxe à utiliser pour créer ces formulaires front-end est assez simple. La configuration se fait pour la plupart grâce à des champs de forulaires cachés.

Deux choses importantes cependant:

- Lorsqu'une erreur de validation se produit, l'URL du formulaire est reloadée et une variable `entry` est disponible. L'`entryModel` qui y est lié décrit l'entry soumise par le formulaire.
- Vous pouvez récupérer les valeurs postées depuis cette variable `entry`, ainsi que les erreurs de validations via `entry.getError()`, `getErrors()`, ou `getAllErrors()`.

**Entry form**

```twig
<form method="post" action="" accept-charset="UTF-8">
    {{ getCsrfInput() }}
    <input type="hidden" name="action" value="entries/saveEntry">
    <input type="hidden" name="redirect" value="viewentry/{slug}">
    <input type="hidden" name="sectionId" value="2">
    <input type="hidden" name="enabled" value="1">

    <label for="title">Title</label>
    <input id="title" type="text" name="title" {%- if entry is defined %}value="{{ entry.title }}"{% endif -%}>

    <label for="body">Body</label>
    <textarea id="body" name="fields[body]">{%- if entry is defined %}{{ entry.body }}{% endif -%}</textarea>

    <input type="submit" value="Publish">
</form>
```

**Guest entries**

```twig
<form method="post" action="" accept-charset="UTF-8">
    {{ getCsrfInput() }}
    <input type="hidden" name="action" value="guestEntries/saveEntry">
    <input type="hidden" name="redirect" value="guest/success">
    <input type="hidden" name="sectionId" value="3">

    <label for="title">Title</label>
    <input id="title" type="text" name="title" {%- if entry is defined %}value="{{ entry.title }}"{% endif -%}>

    <label for="body">Body</label>
    <textarea id="body" name="fields[body]">{%- if entry is defined %}{{ entry.body }}{% endif -%}</textarea>

    <input type="submit" value="Publish">
</form>
```

### Utiliser Craft comme un headless CMS: Element API plugin

Vous pouvez également utiliser Craft comme un CMS headless, c'est à dire un CMS rendant les contenus accessibles via une API (souvent une API JSON). Dans ce scénario, votre CMS ne se charge pas d'afficher les contenus et n'est pas en charge des templates ou des vues mais est uniquement chargé de créer, supprimer, modifier et organiser vos contenus.

Pixel&Tinic a développé [Element API](https://github.com/pixelandtonic/ElementAPI), un plugin qui vous permet de facilement créer des API endpoints, prêts à être utilisés par votre couche front-end et vos views (Vue.js, Ember, Angular, etc.). A noter également qu'[une API REST complète](http://feedback.craftcms.com/forums/285221-feature-requests/suggestions/8622559-rest-api) devrait être disponible "at some point after Craft 3.0 is released".

### Import de données: plugins

Craft est un CMS relativement jeune et la plupart des projets consistent à redesigner et à relancer des sites existants plutôt qu'à créer un site à partir de rien. Il vous faudra donc souvent importer des données existantes dans une installation Craft.

Heureusement pour nous, il existe quelques bons plugins d'import sur lesquels vous pouvez compter. Mes deux favoris sont:

- [Feed Me par Engram Design](https://github.com/engram-design/FeedMe), qui vous permet d'importer des feeds XML, RSS ou ATOM
- [Import par Bob Olde Hampsink](https://github.com/boboldehampsink/import), qui vous permet d'importer des fichiers CSV

Generalement, j'importe ces données en créant des feeds RSS dans l'ancien système (ce que la plumart des CMS permettent de faire facilement) et j'utilise Feed Me pour importer les nodes comme entries dans Craft. Il reste en génaral un peu de travail à effectuer manuellement mais la plupart des données peuvent être importées automatiquement.

## Exercices

### A faire ensemble

Construire un blog simple.

1. Homepage
  - single section
  - affiche seulement les 3 derniers blogposts
2. Blog archive
  - liste paginée avec 5 posts sur chaque page
  - les blogposts peuvent être filtrés à l'aide de catégories thématiques
3. Blogpost détail
  - les blogposts doivent avoir une image pour que nous puissions pratiquer les transformations d'images
4. APage about
  - single section

### A faire seul

1. Ajouter une page d'archive à votre blog qui permet de visualiser les posts par année en utilisant du dynamic routing.
2. Créer une section "portfolio" vous permettant de présenter vos travaux. Créer une navigation dans les pages de détail permettant de naviguer de page de détail à page de détail.
3. Add a search tool and a search result page

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
- [Manipulating Craft's ElementCriteriaModel objects with Twig](http://www.webstoemp.com/blog/manipulating-craft-elementcriteriamodel-with-twig/): une technique simple mais efficace pour créer des fonctionnalités complexes en quelques lignes de Twig
- [Using Macros to improve consistency in your Craft CMS templates](http://withchief.com/blog/using-macros-to-improve-consistency-in-your-craft-cms-templates): une bonne introduction aux macros par [Jamie Pittock](https://twitter.com/jamiepittock) sur le blog de Chief.
- [Combined search and filters using Craft CMS](http://webstoemp.com/blog/combined-searches-and-filters-craft-cms/): un blogpost de votre serviteur sur la manipulation d'objets dans twig pour créer des recherches et filtres complexes avec Craft CMS.
