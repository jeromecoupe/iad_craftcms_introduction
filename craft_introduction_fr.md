# Craft par Pixel&Tonic: Introduction

## Introduction

### Technologies et ethos

[Craft](http://buildwithcraft.com/) est un CMS créé par [Pixel & Tonic](http://pixelandtonic.com/), développé à l'aide de technologies open source comme PHP et MySQL et s'appuie sur un framework PHP qui a fait ses preuves: [Yii](http://www.yiiframework.com/).

Comparé à d'autres produits, Craft est axé sur l'essentiel: définir et gérer vos contenus, et ce de la façon la plus modulaire et flexible possible. Si vous avez besoin d'un moteur de commentaires, d'un forum ou encore d'e-commerce, il faudra vous tourner vers des plugins.

### Un modèle de pricing modulaire

Cette flexibilité est présente jusque dans [le pricing](https://craftcms.com/pricing) du produit lui même.

- La version "Solo" de Craft est gratuite et vous permet de profiter de toutes les fonctionnalités de Craft hormis la gestion utilisateurs. The seul compte utilisateur disponible est un compte admin.
- Une version "Pro" à 299 USD vous permet de disposer de toutes les fonctionnalités de Craft, y compris la gestion d'utilisateurs, une API GraphQL et la capacité de modifier le branding de l'outil.

#### Licences

Les licences Craft cont perpétuelles, c'est à dire que vous pouvez utiliser la version achetée indéfiniment. Vous achetez également un an de mises à jour et de support avec votre licence.

Si vous en avez besoin, vous pouvez ensuite payer 59 USD à n'importe quel moment pour avoir droit à un an de mises à jour et de support.

#### Versions de test

[Vous pouvez tester Craft Pro](https://craftcms.com/guides/try-craft-pro-plugins-before-buying) et toutes ses fonctionnalités gratuitement pourvu que vous soyez sur un domaine local et identifiable comme tel.

### Une plateforme de e-commerce intégrée

Pixel & Tonic a également sorti [Craft Commerce](https://craftcms.com/commerce), une plateforme de e-commerce pour Craft CMS. Une licence coûte 999 USD et vous donne droit à [une liste impressionnante de fonctionnalités](https://craftcommerce.com/features).

Une version "Lite" de Craft Commerce est également disponible, avec des fonctionnalités plus simples et un prix de \$199 par projet.

Nous ne couvrirons pas Craft Commerce lors de cette introduction à Craft CMS. Sachez que Craft Commerce fait largement appel aux mêmes concepts que Craft, que ce soit pour la création de votre data structure ou au niveau de vos templates. Vous vous sentirez en terrain familier. Si vous cherchez un module e-commerce étroitement intégré avec un CMS, Craft Commerce est certainement une option à considérer.

### Atouts

Les principaux atouts de Craft sont à mon sens:

- Une flexibilité dans la définition de votre data structure rarement égalée. Les 16 field types disponibles par défaut permettent une approche extrêmement modulaire.
- A propos de champs et de modularité, Matrix est un atout de Craft en ce qui concerne la flexibilité de votre data structure tout en préservant un contrôle total sur le code front-end généré.
- [L'utilisation de Twig comme language de templating](http://twig.sensiolabs.org/doc/templates.html): cela induit un temps d'apprentissage mais les gains en termes de puissance, de modularité et de flexibilités sont importants.
- Une quantité impressionnante de fonctionnalités qui rendront la vie de vos clients plus facile: live preview, control panel responsive, one click updates, etc.
- Un découplage important entre votre structure de dossiers et de fichiers et la manière dont sont construites les URL. La structure de vos URL est donc extrêmement flexible.
- Une solution intégrée et complète pour les sites multilingues.
- Une équipe de développement et de support fantastique

## 1. Installation et configuration

### Installation

Après avoir vérifié que votre serveur est capable de faire tourner Craft, il ne vous reste qu'à suivre une [procédure d'installation simple](https://craftcms.com/docs/4.x/installation.html) et à vérifier l'ensemble de vos permissions de dossiers et de fichiers. Cela devrait être assez rapide.

### Version control

La plupart des développeuses / développeurs utilisent Git pour maintenir leur code et en gérer les différentes versions. Craft possède [son propre fichier `.gitignore`](https://github.com/craftcms/craft/blob/main/.gitignore). Il vous reste à gérer vos propres fichiers et dossiers. Voici le `.gitignore` que j'utilise comme base.

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

### Configurations pour environnements multiples

Craft fournit nativement une façon simple de gérer des environnements multiples (dev, staging, production).

Dans la mesure ou vos développeurs utilisent chacun une architecture de dossiers et de fichiers locale différente et parce que des informations sensibles ne doivent pas apparaître dans un repository Git, Craft vous propose d'utiliser un fichier `.env` à la racine de votre projet. Ce fichier vous permet d'utiliser les valeurs spécifiées dans ce fichier `.env` dans `craft/config/general.php` and `craft/config/db.php`.

Voici un exemple simple.

**Exemple**: `.env`

```
# The environment Craft is currently running in (dev, staging, production, etc.)
ENVIRONMENT=dev

# The application ID used to to uniquely store session and cache data, mutex locks, and more
APP_ID=randomid

# The secure key Craft will use for hashing and encrypting data
SECURITY_KEY=randomkey

# Database Configuration
DB_DRIVER=mysql
DB_SERVER=127.0.0.1
DB_PORT=3306
DB_DATABASE=dbname
DB_USER=dbuser
DB_PASSWORD=dbpassword
DB_SCHEMA=public
DB_TABLE_PREFIX=

# The URI segment that tells Craft to load the control panel
CP_TRIGGER=admin

# Base URL and path (no trailing slashes)
BASE_URL = https://myproject.craft.test
BASE_PATH = /Users/username/data/weblocal/myproject
```

Vous pouvez utiliser les valeurs définies dans ce fichier `.env` dans vos fichiers de configuration `craft/config/general.php` et `craft/config/db.php` (vous devrez créer ce fichier si il n'existe pas). Cela vous permet de définir n'importe quel [paramètre de configuration](https://craftcms.com/docs/4.x/config/config-settings.html).

Vous pouvez également utiliser ces valeurs pour créer des [alias Yii](https://craftcms.com/docs/4.x/config/#aliases) utilisables dans le control panel, par exemple pour définir les chemins et URls de vos assets volumes pour les adapter à divers environnements. Vous pouvez également les utiliser dans vos templates via la fonction `alias()` de Craft.

#### Map ou Fluent

Vos fichiers de settings peuvent utiliser deux syntaxe: map ou fluent. Les principaux avantages de la syntaxe "Fluent" sont qu'elle vous permet d'avoir de l'auto-complétion et de la documentation inline.

**Exemple (map)**: `config/general.php`

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

$isDev = App::env('ENVIRONMENT') === 'dev';
$isProd = App::env('ENVIRONMENT') === 'production';

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
    '@web' => App::env('BASE_URL'),
    '@baseUrl' => App::env('BASE_URL'),
    '@basePath' => App::env('BASE_PATH'),
    '@assetsBasePath' => App::env('BASE_PATH').'/uploads',
    '@assetsBaseUrl' => App::env('BASE_URL').'/uploads',
    '@webroot' => App::env('BASE_PATH')
  ]
];
```

**Exemple (fluent)**: `config/general.php`

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

$isDev = App::env('ENVIRONMENT') === 'dev';
$isProd = App::env('ENVIRONMENT') === 'production';

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
    '@web' => App::env('BASE_URL'),
    '@baseUrl' => App::env('BASE_URL'),
    '@basePath' => App::env('BASE_PATH'),
    '@assetsBasePath' => App::env('BASE_PATH').'/uploads',
    '@assetsBaseUrl' => App::env('BASE_URL').'/uploads',
    '@webroot' => App::env('BASE_PATH')
  ])
;
```

**Exemple (map)**: `config/db.php`.

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
  'dns' => App::env('DB_DRIVER') :? null,
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

**Exemple (fluent)**: `config/db.php`.

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
  ->dsn(App::env('DB_DSN') ?: null)
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

**Exemple**: utilisation des alias dans la configuration de vos assets volumes (locaux).

```
@assetsBasePath/partners_logos/
@assetsBasePath/partners_logos/
```

**Exemple**: utilisation des alias dans vos templates avec la fonction `alias()`.

```twig
{% if alias('@environment') == 'production' %}
  {# include Google Analytics code #}
{% endif %}
```

```twig
<link rel="stylesheet" href="{{ alias('@baseUrl') }}/dist/css/main.min.css">
```

Les valeurs définies via `dotenv` et utilisées dans une configuration de production doivent être disponibles pour Craft sur votre serveur de production. En général, cela est fait via la configuration de votre serveur web, que ce soit Apache ou Nginx. Les variables `.env` sont parfois déconseillés en production et les hébergeurs vous offrirons souvent un moyen de les configurer au niveau du serveur.

### Editeurs HTML

Craft vous permet d'utiliser [CKEditor](https://ckeditor5.github.io/) ou [Redactor](http://imperavi.com/redactor/) comme solution WYSIWYG pour permettre à vos utilisateurs d'utiliser du texte formaté en HTML. Les deux sont disponibles comme Plugins gratuits dans le Plugins Store de Craft.

Pour ma part, j'ai toujours utilisé Redactor avec des configurations très simples.

Les configurations de Redactor peuvent facilement être créées et modifiées à l'aide de fichiers JSON dans le dossier `config/redactor/` une fois le plugin installé. Les noms de vos fichiers JSON seront les noms des configurations disponibles dans votre Control Panel.

## 2. Définir et structurer vos données

Craft vous permet de structurer les données de votre site de façon extrêmement flexible et modulaire.

### Sections, Entries et entry types

Avec Craft, vos contenus vont principalement "vivre" dans des entries, elles-même contenues dans des sections.

La data structure de ces entries va être déterminée par les custom fields que vous ajouterez à ces sections. Pour chaque entry type dans Craft, vous pouvez créer un field layout qui va préciser quels fields vont être utilisés pour définir les entries de cette section.

Il y a [trois grands types de sections](https://craftcms.com/docs/4.x/entries.html) dans craft: singles, channels et structures.

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

Pour chaque entry type disponible vous pouvez spécifier un field layout: assigner à votre entry type des custom fields qui vont définir la data-structure de toutes les entries de ce type.

L'entry type peut facilement être utilisé [dans le routing et les patterns d'URL](https://craftcms.com/knowledge-base/entry-type-urls), comme [dans vos tags `craft.entries()`](https://craftcms.com/docs/4.x/fields.html) et vos tests conditionnels.

#### Sections de type structure

Les sections de type structure ressemblent beaucoup aux sections de type channel: elles peuvent contenir plusieurs entry types et les patterns d'URL des entries qu'elles contiennent peuvent être précisés.

La grande différence est que ces sections de type structure sont destinées à créer des agencements hiérarchiques d'entries dont l'ordre peut être modifié manuellement. Vous pouvez également préciser le nombre de niveaux possibles dans cette hiérarchie d'entries, les patterns d'URL à utiliser suivant les niveaux des entries dans la hiérarchie, ainsi que le template à utiliser pour les entries de la section.

Via l'écran de configuration de la section vous pouvez préciser:

- le format des URL des entries de la section. Ce format peut être différent pour les entrées de niveau 1 ou pour les entries imbriquées.
- le template à utiliser pour le rendu des entries de la section

Pour chaque entry type disponible vous pouvez spécifier:

- un field layout permettant d'assigner à vos entry type des custom fields qui vont définir la data-structure de toutes les entries de ce type

### Fields, Field Groups et Field Layouts

Craft vous propose [de nombreux types de champs](https://craftcms.com/docs/4.x/fields.html) à l'aide desquels vous pouvez définir la data structure de vos entries.

Dans Craft, un champ peut être appliqué à n'importe quel nombre d'entries, de users, de tags, d'assets, de categories ou de globals via un "field layout" qui permet d'effectuer toutes les opérations sur les fields dans une interface "drag and drop".

Les fields peuvent être groupés au sein de groupes. Ces groupes n'ont qu'une fonction organisationnelle. Créer des groupes permet de gérer plus facilement un grand nombre de champs.

### Globals

A côté des sections et des entries, les [globals](https://craftcms.com/docs/4.x/globals.html) peuvent être utilisées pour stocker du contenu. Les globals sont en utilisées pour des contenus brefs, qui n'ont pas leur place dans des entries mais qui doivent pouvoir être édités facilement via le control panel: tagline, coordonnées de contact, code Google Analytics, etc.

Vous pouvez créer des groupes de contenus en utilisant les global sets. Chaque set de globals possède son field layout et donc sa propre data structure.

### Users

Dans Craft, les [utilisateurs](https://craftcms.com/docs/4.x/users.html) servent à gérer les permissions données aux divers utilisateurs du système.

Les utilisateurs peuvent être assignés à divers groupes. Ces groupes sont utilisés pour gérer les permissions données aux utilisateurs qui en sont membres. Un utilisateur peut être assigné à plusieurs groupes.

La data structure des utilisateurs peut être aussi complexe que souhaitée. Via un unique Field layout, les champs souhaités peuvent être assignés à la data structure de tous vos utilisateurs.

### Assets

Les [assets](https://craftcms.com/docs/4.x/assets.html) vous permettent de gérer vos fichiers Craft. Vos Assets sont liés à des Asset Volumes (permettant de gérer les custom fields) et à des Assets et des Transform Filesystems qui correspondent à des dossier sur votre serveur ou à des services d'hébergement externes dans le cloud tels que Rackspace ou Amazon S3.

De multiples [transformations d'images](https://craftcms.com/docs/4.x/image-transforms.html) peuvent être appliquées automatiquement sur vos assets (crop, scale, quality, etc.), soit via le control panel, soit via vos templates Twig. Ces transformations vous permettent de créer les thumbnails nécessaires pour votre site à partir d'images de base.

Un field layout est associé à chaque Asset Volume et vous permet de créer une data-structure en y d'associant des custom fields. Vos documents peuvent donc avoir une data-structure différente de vos photos par exemple.

### Tags

Les [tags](https://craftcms.com/docs/4.x/tags.html) vous permettent de créer des _folksonomies_ et de les appliquer à vos Entries, Users ou Assets.

Chaque tag doit être assigné à un groupe et chaque groupe de tags possède un field layout. Vous pouvez donc créer des structures de données assez complexes pour chacun de vos groupes de tags.

### Categories

Les [categories](https://craftcms.com/docs/4.x/categories.html) vous permettent de créer des taxonomies et de les appliquer à vos Entries, Users ou Assets.

Chaque catégorie doit être assignée à un groupe.

- Chaque groupe possède un field layout.
- Au sein de chaque groupe vous pouvez spécifier la structure des URLs pour vos catégories et vos sous-categories
- Pour chaque groupe de categories, vous pouvez spécifier le template qui va être chargé quand une URL de catégorie est demandée.

### Relations

L'une des grandes forces de Craft c'est qu'il est possible très facilement de créer des [relations](https://craftcms.com/docs/4.x/relations.html) entre Entries, Users, Assets et Tags.

Pour cela, Craft met à votre disposition des champs relationnels spécifiques:

- **Assets**: permet d'établir une relation "one to one" ou "one to many" vers des Assets
- **Entries**: permet d'établir une relation "one to one" ou "one to many" vers des Entries
- **Users**: permet d'établir une relation "one to one" ou "one to many" vers des Users
- **Tags**: permet d'établir une relation "one to one" ou "one to many" vers des Tags
- **Categories**: permet d'établir une relation "one to one" ou "one to many" vers des Catégories

Pour chacun des ces tags, vous pouvez spécifier combien d'items peuvent être liés et de quelles sources ils proviennent.

Pour exploiter ces relations dans vos templates, Craft met à votre disposition un outil extrêmement puissant: le paramètre [`relatedTo`](https://craftcms.com/docs/4.x/relations.html#the-relatedto-parameter), utilisable avec `craft.entries()`, `craft.users()`, `craft.assets()`, `craft.tags()` et `craft.categories()`.

### Routing

L'un des autres éléments intéressant de Craft c'est le [routing](https://craftcms.com/docs/4.x/routing.html) dynamique, qui permet de séparer structure des URLs et architecture de dossiers et de fichiers.

Comme nous l'avons déjà vu, Craft cous permet de spécifier une structure d'URL propre pour chaque Entry, User, Asset, Tags and Categories.

Si cela ne suffit pas à couvrir tous vos besoins, vous pouvez également créer des routes dynamiques. Pour chaque route créée, vous pouvez spécifier quel template doit être chargé par Craft.

Il est possible créer des routes et de spécifier quel template doit être chargé par Craft. Un exemple facile à comprendre est [un template donnant accès à une archive des entries classées par année](https://craftcms.com/guides/creating-an-archive-page-for-entries#yearly-archive-pages).

En créant une route dynamique `blog/archive/{year}` renseignant le template `blog/archive`, les URL `blog/archive/2013` et `blog/archive/2012` vont charger le même template et rendre disponible une variable `year` utilisable par Twig et par Craft avec les paramètres [`after`](https://craftcms.com/docs/4.x/entries.html#after) et [`before`](https://craftcms.com/docs/4.x/entries.html#before).

Si vous avez besoin de plus de possibilités que celles offertes par le control panel, sachez que [les routes peuvent également être gérées via un fichier](https://craftcms.com/docs/4.x/routing.html#advanced-routing-with-url-rules) `config/routes.php`, ce qui vous donne accès à du matching d'URL en utilisant des expressions régulières.

## 3. Twig comme language de templating

Craft utilise [Twig](http://twig.sensiolabs.org/), créé par Fabien Potencier pour Symfony, comme language de templating. Twig a l'avantage de compiler les templates en PHP, ce qui lui permet d'être très performant. C'est un language qui reste également assez simple d'approche, même si une période d'apprentissage existe.

Couplé à des tags, fonctions et filtres spécifiques à Craft, Twig vous permet de récupérer et de manipuler vos données au sein de vos templates. Pour une introduction rapide à Twig, vous pouvez [consulter les slides de Brandon Kelly sur Slideshare](http://www.slideshare.net/brandonkelly212/twig-for-designers).

### Principaux tags dans Twig

En plus des [tags disponibles dans Twig](http://twig.sensiolabs.org/doc/tags/index.html), Craft possède également quelques tags qui lui sont propres. Nous reviendrons sur certains d'entre-eux dans la suite du cours.

Twig possède trois grands types de tags:

- `{# Commentaires #}`
- `{{ Affichage }}`
- `{% Execution / Logique %}`

#### Tags de commentaires

Twig possède également un tag de commentaire: `{# Ceci est un commentaire #}`. Ceux-ci ne sont pas affiché lorsque le template est affiché.

#### Tags d'affichage, variables et propriétés

`{{ Affichage }}`: ces tags vous permettent d'afficher des chaines de caractère, nombres, booléens, tableaux et objects dans vos templates. La plupart du temps, vous afficherez des variables créées par vous ou par Craft. Une notation pointée permet d'accéder aux propriétés de ces variables.

Exemples:

- `{{ "Hello World" }}`: affiche la chaîne de caractères "Hello World"
- `{{ entry.title }}`: affiche la propriété title de l'objet entry
- `{{ 8 + 2 }}`: affiche 10

#### Tags d'exécution ou de logique

`{% Execution / Logique %}`: ces tags permettent l'exécution de tâches et sont utilisés pour créer des variables, réaliser des boucles, créer des structures de contrôle, etc.

Exemples:

Créer une variable `allEntries` à laquelle est assignée le résultat d'une [ElementQuery](https://craftcms.com/docs/4.x/element-queries.html) contenant les 10 dernières entries dans la section blog, classées par date de création en ordre descendant.

```twig
{% set allEntries = craft.entries()
  .section('blog')
  .limit(10)
  .order('postDate desc')
  .all() %}
```

Boucler sur le tableau d'objets `allEntries` en créant à chaque fois un objet `entry` dont nous affichons le titre.

```twig
{% for entry in allEntries %}
  <p>{{ entry.title }}</p>
{% endfor %}
```

Créer une [structure de contrôle](http://twig.sensiolabs.org/doc/templates.html#control-structure) vérifiant si la variable `allEntries` contient au minimum une entry.

```twig
{% if allEntries|length %}
  <p>There is at least one entry here</p>
{% endif %}
```

#### Jamais de tags à l'intérieur d'autres tags

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

### Types de données et variables dans Twig

Twig supporte 5 grands types de données:

- Strings: `"Hello"` ou `'Hello'`
- Numbers: `4` ou `3.1498`
- Booleans: `true` ou `false`
- Arrays: `['orange', 'lemon', 'apple']`
- Objects: `{ foo: 'bar' }`

Comme dit plus haut, vous pouvez assigner une valeur à une variable dans Twig en utilisant le tag `{% set %}`

```twig
{% set firstName = "Jérôme" %}`
{% set allEntries = craft.entries()
  .section('blog')
  .order('postDate desc')
  .all() %}
```

### Filtres

[Twig](http://twig.sensiolabs.org/doc/filters/index.html) et [Craft](https://craftcms.com/docs/4.x/dev/filters.html) possèdent des filtres qui peuvent être appliqués à vos différents types de variables pour les modifier. Ces filtres offrent de nombreuses possibilités et permettent à Craft de se passer de nombreux plugins pour effectuer des tâches simples. Voici quelques exemples de ce qu'il est possible d'accomplir:

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
{% set allEntries = craft.entries()
  .section('blog')
  .order('postDate desc')
  .all() %}
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

### Fonctions

Les [fonctions disponibles dans Twig](http://twig.sensiolabs.org/doc/functions/index.html) permettent de produire et de manipuler des contenus. Craft possède également [ses propres fonctions](https://craftcms.com/docs/4.x/dev/functions.html), en plus de celles fournies par Twig.

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

`dump()` est une fonction extrêmement utile disponible lorsque [Dev Mode](https://craftcms.com/guides/what-dev-mode-does) est activé. Utile pour le debugging.

```twig
{{ dump(entry) }}
```

### Structures de contrôle et conditionnels

Twig vous permet de créer des structures de contrôles à l'aide de conditionnels.

```twig
{% if allEntries|length %}
  <p>There is at least one entry here</p>
{% endif %}
```

**If / else**

```twig
{% if currentUser.admin %}
  <p>Welcome, master<p>
{% else %}
  <p>What can I do for you, dear user?</p>
{% endif %}
```

**Conditions imbriquées**

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

**Conditions cumulées**

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

### Expressions Mathématiques et manipulation de chaînes de caractères

Twig est capable d'interpréter toutes sortes d'[opérations mathématiques](http://twig.sensiolabs.org/doc/templates.html#math) et de manipuler des chaînes de caractères (strings).

```twig
{{ 10 * (8+2) }}
{{ "Hello World"|slice(0,5) }}
```

### Contrôle du whitespace

Twig vous permet de contrôler la façon dont votre code est affiché, [particulièrement au niveau du whitespace](http://twig.sensiolabs.org/doc/templates.html#whitespace-control).

### Template inheritance, includes et macros: stay DRY

Ces trois concepts sont au coeur de Twig et vont nous permettre de créer des templates efficaces et évitant au maximum les redondances inutiles. C'est le fameux principe du "Don't Repeat Yourself" (DRY).

#### Template inheritance

Un concept central à comprendre dans Twig est celui d'héritage au niveau des templates. Dans Twig on va généralement travailler avec un template "parent" qui défini le squelette de la page et différents blocs pouvant être surdéterminés par un template "enfant" qui étend ce template parent.

Les variables définies dans le template "enfant" sont accessibles dans le template parent.

Voyons voir comment cela fonctionne dans la pratique avec un exemple simple:

**Template parent: layouts/\_base.html**

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

**Template enfant: news/index.html**

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

Le template `news/index.html` étend notre layout de base. Le contenu défini dans le block "content" du template enfant remplace celui qui est (de façon optionnelle) défini dans le template parent. La variable `htmlTitle` définie dans le template enfant est accessible dans le template parent.

Remarquez également que nous écrivons le nom du template "parent" précédé par un underscore. Craft nous permet ainsi de cacher certains templates que nous ne voulons pas voir chargé directement par les visiteurs du site.

Typiquement, vos layouts, includes et template de détail pour vos entry sont tous rendus inaccessibles de cette façon.

#### Includes

Si vous avez du code qui est répété dans beaucoup de vos templates, vous pouvez également utiliser le tag `{% include %}` qui vous permet d'inclure un template au sein d'un autre.

```twig
{% include 'sidebars/_default.html' %}
```

### Macros

[Les Macros](http://twig.sensiolabs.org/doc/tags/macro.html) dans Twig sont comparables à des mixins en Sass. Pensez à elles comme à de petits blocs de code réutilisables.

Une macro est définie à l'aide des tags `{% macro %}` et `{% endmacro %}`, soit dans un fichier externe, soit dans le même fichier dans lequel elle est utilisée.

```twig
{% macro dateText(date) %}
  {{ date|date('F j, Y') }}
{% endmacro %}

{% macro dateNumeric(date) %}
  {{ date|date('d.m.Y') }}
{% endmacro %}
```

Les macros sont appelées / importées à l'aide du tag `{% import %}`

```twig
{% import "_macros/dates" as dateHelpers %}
{{ dateHelpers.dateText(entry.postDate) }}
```

## 4. Récupérer et manipuler vos données avec Craft et Twig

Avec Craft, vous interagissez avec la base de données en utilisant des [Element Queries](https://craftcms.com/docs/4.x/element-queries.html). Cela à l'air très compliqué mais c'est en fait un concept assez simple:

1. vous créez une ElementQuery pour le type de données que vous souhaitez récupérer dans la base de données (entries, users, assets, etc.)
2. vous spécifiez les paramètres à utiliser (limit, order, filters, etc.)
3. Vous exécutez l'ElementQuery en utilisant les fonctions `.all()`, `one()`, `exists()`, `.count()` ou `.ids()`
4. Craft retourne un element ou un array d'elements ([entry](https://docs.craftcms.com/api/v4/craft-elements-entry.html), [user](https://docs.craftcms.com/api/v4/craft-elements-user.html), [asset](https://docs.craftcms.com/api/v4/craft-elements-asset.html), [category](https://docs.craftcms.com/api/v4/craft-elements-category.html) ou [tag](https://docs.craftcms.com/api/v4/craft-elements-tag.html)).
5. Vous pouvez ensuite afficher ces objets ou array d'objets dans vos templates.

`craft.entries()`, `craft.users()`, `craft.assets()`, `craft.categories()` et `craft.tags()` seront vos principaux outils de travail. Nous nous centrerons ici principalement sur `craft.entries()`. Les autres tags ayant un fonctionnement très similaire, il vous sera facile d'appliquer les mêmes principes.

### Entries

[`craft.entries()`](https://craftcms.com/docs/4.x/entries.html#querying-entries) est le tag que vous allez utiliser pour récupérer vos entries.

- `craft.entries().all()` vous permet de récupérer toutes les entries qui correspondent à vos critères
- `craft.entries().one()` vous permet de récupérer la première entry qui correspond à vos critères (retourne `null` si aucune entry ne correspond). Si vous souhaitez récupérer la dernière entry correspondant à vos critères, vous pouvez utiliser `craft.entries().inReverse().one()`
- `craft.entries().exists()` vous permet de vérifier si au moins une entry correspond à vos critères (retourne `true` or `false`).
- `craft.entries().count()` vous permet de récupérer le nombre total des entries correspondant à vos critères.
- `craft.entries().ids()` vous permet de récupérer la liste des ids des entries correspondant à vos critères.

Depuis la version 4 de Craft, nous pouvons également utiliser `.collect()` pour récupérer nos éléments sous la forme de collections Laravel. Celles-ci permettent:

1. De simplifier / de standardiser notre code lorsque nous utilisons de l'eager loading: nous pouvons utiliser une syntaxe pointée au mieu de devoir passer à une syntaxe propre aux tableaux.
2. D'accéder à une série de [méthodes permettant de les manipuler](https://laravel.com/docs/9.x/collections#available-methods) héritées de Laravel.

#### ElementQuery simple

Voici un exemple d'Element Query simple.

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

Il est possible de créer des Element Queries plus complexes en utilisant des [Element Queries avancées](https://craftcms.com/docs/4.x/element-queries.html#advanced-element-queries).

#### Pas de résultats

Vous pouvez facilement afficher un contenu alternatif si aucun résultat n'est trouvé en [utilisant une clause {% else %} dans votre loop](http://twig.sensiolabs.org/doc/tags/for.html#the-else-clause).

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

#### "Loop", "cycle" et "is divisible by"

Lorsque une boucle `{% for %}` est utilisée, il est souvent très pratique de pouvoir évaluer à quelle étape de la boucle on se trouve et d'utiliser des conditionnels. Typiquement, il est utile de pouvoir ouvrir et fermer une liste `<ul>` en début ou en fin de loop, de pouvoir afficher quelque chose tous les x résultats. C'est à cela que servent la variable [`loop`](http://twig.sensiolabs.org/doc/tags/for.html#the-loop-variable), la fonction [`cycle`](http://twig.sensiolabs.org/doc/functions/cycle.html) et le test [`is divisibleby`](http://twig.sensiolabs.org/doc/tests/divisibleby.html) de Twig.

**Exemple**: utilisation de la variable `loop`

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

**Exemple**: utilisation de la fonction `cycle` pour ajouter des classes `odd` et `even`. Remarquez l'usage de `loop.index0` pour avoir une itération indexée sur zéro et non sur 1 comme avec `loop.index`.

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

Les [autres tests disponibles avec Twig](http://twig.sensiolabs.org/doc/tests/index.html) valent également la peine d'être consultés, notamment le test `is defined`. Les tests disponibles dans Twig sont:

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

Craft vous permet de [paginer vos résultats](https://craftcms.com/docs/4.x/dev/tags.html#paginate) à l'aide du tag `{% paginate %}` et de construire une interface de pagination à l'aide des variables qui l'accompagnent. Attention, le tag `{% paginate %}` nécessite un objet ElementQuery comme paramètre. N'utilisez simplement pas la fonction `all()` dans ce cas-ci.

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

Le même principe est d'application lorsqu'une page de catégorie est affichée. Lorsqu'une URL définie comme une URL de catégorie est affichée par le système, Craft défini automatiquement une variable `category` que vous pouvez utiliser directement au sein de vos templates.

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
  # - when a category route is called, an 'enrty' variable is not created
  # - we create the entry variable by hand if not defined
  #}

  {% if entry is not defined %}
    {% set entry = craft.entries().id(7).one() %}
  {% endif %}

  {#
   # - craft automatically creates a 'category' variable if it detects you are on a category template
   #  - we are just checking whether that category variable exists or not
   #  - depending on its existence, we set our list of entries
   #}

  {% set allCategories = craft.categories().group('newsTopics').all() %}

  {% set allNews = craft.entries().section('news').limit(10) %}

  {% if category is defined %}
    {% set currentCategory = category.slug %}
    {% do allNews.relatedTo(category) %}
  {% else %}
    {% set currentCategory = 'all' %}
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

Les globals stockent du contenu qui va, comme leur nom l'indique, être disponible globalement pour tous les templates.

Vous pouvez y accéder très facilement via leur handle de global set suivi de leur handle de global. Par exemple, pour une global appelée `tagline` dans un global set `companyInfo`:

`{{ companyInfo.tagline }}`

### Tags

Dans Craft, on accède aux tags avec `craft.tags()`, qui [possède un certain nombre de paramètres](https://craftcms.com/docs/4.x/tags.html#parameters) et fonctionne dans l'ensemble comme `craft.entries()` mais retourne un objet ou un array d'objets [`Tag`](https://docs.craftcms.com/api/v4/craft-elements-tag.html).

Deux articles sur vous montrent comment obtenir une [liste de tous les tags utilisés par les entries d'une section](https://craftcms.com/guides/displaying-tags-that-are-in-use), ou encore comment créer, à l'aide d'une route dynamique, [une page d'archive reprenant toutes les entries liées à un tag](https://craftcms.com/guides/assigning-urls-to-tags).

### Users

Le tag `craft.users()` permet d'accéder aux utilisateurs de votre site. Ce tag possède lui aussi [un certain nombre de paramètres, dont certains lui sont propres](https://craftcms.com/docs/4.x/users.html#parameters).

Son fonctionnement est semblable au tag `craft.entries()` mais il retourne un objet ou un array d'objets [`User`](https://docs.craftcms.com/api/v4/craft-elements-user.html)).

### Assets et transformations

Le tag `craft.assets()` permet d'accéder aux Assets de votre site. Ce tag possède lui aussi [un certain nombre de paramètres, dont certains lui sont propres](https://craftcms.com/docs/4.x/assets.html#parameters).

Son fonctionnement est semblable au tag `craft.entries()` dans la mesure où il retourne un objet [`Asset`](https://docs.craftcms.com/api/v4/craft-elements-asset.html).

Si vos assets sont des images, Craft vous permet d'effectuer des transformations de celles-ci, soit à l'upload (via votre control panel: Settings > Assets > Image Transforms), soit dynamiquement dans vos templates.

Si vous définissez une transformation directement dans le control panel et que vous lui donnez comme handle `thumbnail` vous pouvez y accéder dans vos templates de la façon suivante :

```twig
{% if myAssetField | length %}
  {% set heroImage = myAssetField.one() %}
  <img src="{{ heroImage.getUrl('thumbnail') }} width="{{ asset.getWidth('thumbnail') }}" height="{{ asset.getHeight('thumbnail') }}" alt="{{ heroImage.title }}">
{% endif %}
```

Vous pouvez également définir dynamiquement une transformation dans vos templates, ce qui est plus explicite.

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

[Matrix](https://craftcms.com/features/all#matrix) est un type de champ particulièrement intéressant et puissant dont vous disposez dans Craft. Il vous permet de créer des champs qui combinent différents blocs et de définir la structure pour chacun de ces blocs.

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

Au niveau du templating, vous pouvez également contrôler très précisément le code HTML généré.

Nous utilisons ici [le tag `{% switch %}` propre à Craft](https://craftcms.com/docs/4.x/dev/tags.html#switch).

```twig
{# Modular Body #}
{% for module in entry.modularBody %}

  {% switch module.type %}

    {% case "textModule" %}

      {{ module.textContent }}

    {% case "quoteModule" %}

      <blockquote>
        <p>{{ module.quoteText }}</p>
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

Personnellement, je préfère simplifier mes templates et placer l'ensemble des vues pour mes Matrix Blocks dans des fichiers dédiés indépendants et auto-suffisants.

Ces fichiers sont auto-suffisants dans la mesure où ils contiennent les transformation d'images ou les les autres variables utilisées.

Cela permet de réutiliser ces template de blocs ailleurs si besoin est.

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

### Recherche

Craft possède un [système de recherche très puissant](https://craftcms.com/docs/4.x/searching.html) basé sur un paramètre `search` utilisable avec les tags `craft.entries()`, `craft.users()`, `craft.assets()` et `craft.tags()`.

Pour des questions de performance, Craft fonctionne avec des Index pour ses fonctions de recherche. Il est possible d'actualiser ces Index directement depuis le Control Panel.

La construction de [formulaires de recherche dynamiques pour le front-end](https://craftcms.com/knowledge-base/search-form) de votre projet est également très simple. Il suffit pour cela de faire appel à `craft.app.request.getParam('parameter')` pour récupérer un paramètre passé en GET/POST par votre formulaire.

### Créer des queries complexes

Comme nous l'avons vu plus haut, les principaux tags de Craft comme par exemple `craft.entries()` acceptent des objets comme paramètres. Twig permet facilement de créer et de manipuler des objets à l'aide de filtres tels que `merge` et `slide`. Craft possède également des filtres propres tels que `without` et `intersect` qui s'avèrent bien utiles.

En combinant ces deux éléments, il devient possible de créer des [queries complexes](https://www.webstoemp.com/blog/modular-element-queries-craft-cms/) et de construire des fonctionnalités relativement avancées assez facilement.

Voici un exemple simple. Vous avez laissé la possibilité à vos utilisateurs de choisir 3 blogposts à afficher sur la homepage. Vous avez donc créé un champs entries dont vous avez spécifié la limite à 3, puisque votre design de la homepage possède seulement 3 emplacements. Vous souhaitez que ces trois emplacements soient toujours remplis. Si l'utilisateur a choisi 1, 2 ou 3 blogposts à l'aide du champs entries, vous voulez afficher ces blogposts d'abord et compléter éventuellement les emplacements restants avec les blogposts les plus récents. Aucun blogpost ne peut être affiché deux fois. Voici comment faire:

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

{% set blogposts = craft.entries()
  .section('blogposts')
  .id(blogpostsIds).fixedOrder(true)
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

### Interfaces de navigation en utilisant un channel de type structure

Si vous essayez d'avoir une entry (single) pour chaque page de votre site, Craft permet facilement de disposer d'une navigation principale flexible et pouvant même être mise à jour par les éditeurs du site. J'utilise généralement une section de type structure pour fournir ce genre de fonctionnalité. Voici un exemple avec une navigation à un seul niveau. Il suffit de créer une section de type structure `mainnav` et deux champs: `mainnavLabel` (textfield) et `mainnavLink` (entries, limité à 1 entry et à des entries de type single). Une simple boucle `for` suffit à créer notre navigation.

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

C'est évidemment un cas assez simple mais cette méthode peut être utilisée pour bon nombre d'interfaces de navigation. Cette technique commence à poser problème lorsque vous avez une navigation de plus de deux niveaux.

### Sites multilingues

Les sites multilingues sont en général assez complexes. Craft rend les choses plus faciles dans la mesure où le multilingue est géré nativement. Les bases sont assez simples et sont détaillée dans la [section Localization](https://craftcms.com/docs/4.x/sites.html) de la documentation.

Je vis et je travaille en Belgique, un pays qui compte trois langues officielles. les capacités de Craft à gérer les sites multilingues sont une bouffée d'air frais. Par la force des choses, j'ai travaillé sur un certain nombre de sites multilingues. Voici [un blogpost détaillant les différentes techniques et macros](https://webstoemp.com/blog/craft-multilingual-websites-tips/) que j'utilise le plus souvent pour ce genre de projets.

Voici également comment construite un language switcher simple.

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

### Optimisation de queries et eager-loading

Un problème courant avec les bases de données est connu sous le nom de "problème n+1". Ce problème se pose lorsque vous devez traverser une collection d'objets ayant des relations avec d'autres: pour chaque objet dans la collection, `n + 1` queries sont générées puisque chaque objet de la collection peut être lié à `n` objets. Un exemple simple avec Craft consiste à demander une série d'entries, chaque entry ayant un asset lié. Lorsque Craft charge ces entries, il va créer `n` queries additionnelles pour vérifier si un asset lié existe ou pas. C'est le comportement par défaut de Craft, qui porte le nom de "lazy loading".

"[Eager-loading](https://craftcms.com/docs/4.x/dev/eager-loading-elements.html)" est une façon d'indiquer à Craft que lorsqu'il va chercher ces entries, chacune d'entre elles possède également un asset qu'il faudra aller chercher lui aussi. Une fois prévenu, Craft va aller chercher les entries et les assets liés en utilisant le moins de queries MySQL possible. Pour faire de l'eager loading, il suffit d'utiliser le paramètre `with` dans vos tags `craft.entries()` par exemple.

**Lazy-loading (assets):**

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

**Eager-loading (assets):**

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

Comme dit plus haut, utiliser `collect()` au lieu de `.all()` ou `.one()` renvoie une collection Laravel, ce qui nous permet de garder la même syntaxe que nous utilisions l'eager-loading ou pas.

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
  .all() %}

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

Utiliser de l'eager loading peut devenir plus complexe. Vous pouvez utiliser l'eager loading avec entries, assets, catégories, tags ou users. Vous pouvez également l'utiliser avec des assets transforms et des Matrix Blocks. Vous pouvez également utiliser l'eager loading pour charger des éléments liés imbriqués, ce qui m'a déjà coûté quelques cheveux blancs. [Un excellent guide concernant l'eager loading](https://straightupcraft.com/articles/examples-of-eager-loading-elements-in-twig-and-php) est disponible sur Straight Up Craft si vous voulez vous pencher d'avantage sur la question.

Voici néanmoins un exemple plus complexe à utiliser si chaque entry possède un champs Matrix qui contient un champs asset pour lequel un transform nommé `thumbnail` est défini dans le template et appliqué:

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

### Le tag `{% cache %}`

[Le tag `{% cache %}`](https://craftcms.com/docs/4.x/dev/tags.html#cache) peut être utilisé pour améliorer la performance de certaines parties de templates. Lorsque le template est chargé pour la première fois, les parties de templates cachées vont exécutées les queries nécessaires pour récupérer les éléments souhaités et vont ensuite stocker l'HTML produit dans la base de données. Lorsque ce template est chargé après cette requête initiale, Craft va seulement chargé le HTML stocké dans la base de données au lieu d'exécuter à nouveau l'ensemble des queries.

Craft va automatiquement effacer les caches lorsque des éléments compris entre les tags `{% cache %}` et `{% endcache %}` sont supprimés ou mis à jour. Vous pouvez également spécifier la durée de vie de vos caches. La durée par défaut si vous ne spécifiez pas une valeur pour le paramètre `for` est celle spécifiée par le paramètre de configuration [`cacheDuration`](https://craftcms.com/docs/4.x/config/config-settings.html#cacheduration). Sa valeur par défaut est d'un jour. Vous pouvez également faire en sorte que vos caches n'expirent jamais à moins qu'un élément qu'ils contiennent soit créé, supprimé ou modifié en mettant la valeur de `cacheDuration` à `false`. Spécifier une valeur pour le paramètre `for` prendra toujours le pas sur cette configuration.

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

Le caching doit idéalement se faire sur des templates déjà optimisés. Une couche de caching appliquée sur un template qui n'est pas optimisé est un emplâtre sur une jambe de bois.

Bien utilisé, le caching vous fera gagner pas mal de performance. Le tag `{% cache %}` est principalement utilisé lors des cas de figures suivants:

- Longues listes d'entries
- Des champs Matrix comportant des champs relationnels (entries, assets, users, categories, tags)
- Affichage de données provenant d'un site tiers (Twitter, etc.)

Vous pouvez tester l'efficacité de vos stratégies de caching en activant le `devMode`, en supprimant les caches dans le control panel (paramètres, Outils) et en rafraîchissant votre page en consultant la Console dans les outils de développement de votre navigateur. Regardez le "profiling summary report", qui vous donnera le nombre total de queries exécutées par Craft et le temps de rendu de la page.

Lors du premier refresh de la page après avoir vidé les caches, toutes les queries seront exécutées et le HTML généré sera enregistré dans la base de données. Lorsque vous rafraichirez la page pour la seconde fois, Craft ne fera que récupérer le HTML et vous devriez constater une baisse importante du nombre de queries et du temps de rendu de la page.

### Formulaires front-end: entry form et guest entries

Par défaut, Craft permet la création d'[entry forms](https://craftcms.com/knowledge-base/entry-form) pour le front-end de votre site. Ces formulaires peuvent uniquement être utilisés par des utilisateurs enregistrés. Vous pouvez également autoriser des utilisateurs anonymes à poster des entries en utilisant le plugin [Guest Entries](https://github.com/craftcms/guest-entries) développé par Pixel&Tonic. Ce plugin vous permet de choisir pour quelles sections vous souhaitez autoriser des entries anonymes et quel auteur par défaut doit être spécifié pour ces entries.

La syntaxe à utiliser pour créer ces formulaires front-end est assez simple. La configuration se fait pour la plupart grâce à des champs de formulaire cachés.

Deux choses importantes cependant:

- Lorsqu'une erreur de validation se produit, l'URL du formulaire est rechargée et une variable `entry` est disponible. L'`entryModel` qui y est lié décrit l'entry soumise par le formulaire.
- Vous pouvez récupérer les valeurs postées depuis cette variable `entry`, ainsi que les erreurs de validations via `entry.getErrors()`, `entry.getFirstError()`, etc.

### Utiliser Craft comme un CMS headless

Vous pouvez également utiliser Craft comme un CMS headless, c'est à dire un CMS rendant les contenus accessibles via une API. Dans ce scénario, votre CMS ne se charge pas d'afficher les contenus et n'est pas en charge des templates ou des vues mais est uniquement chargé de créer, supprimer, modifier et organiser vos contenus.

Craft possède nativement une [API de contenu en GraphQL](https://craftcms.com/docs/4.x/graphql.html) que vous pouvez utiliser avec n'importe que Static Site Generator ou framework de SPA. Le [tutoriel officiel de Craft](https://craftcms.com/docs/getting-started-tutorial/build/graphql.html) et la [documentation](https://craftcms.com/docs/4.x/graphql.html) contiennent quelques principes et guides pour utiliser Craft comme un CMS headless avec GraphQL.

### Import de données: plugins

Craft est un CMS relativement jeune et la plupart des projets consistent à redesigner et à relancer des sites existants plutôt qu'à créer un site à partir de rien. Il vous faudra donc souvent importer des données existantes dans une installation Craft.

Heureusement pour nous, il un excellent plugins d'import sur lequel vous pouvez compter: [Feed Me par Pixel and Tonic](https://plugins.craftcms.com/feed-me), qui vous permet d'importer des données structurées en divers formats (XML, RSS, ATOM, CSV, JSON).

Généralement, j'importe ces données en créant des feeds RSS dans l'ancien système (ce que la plupart des CMS permettent de faire facilement) et j'utilise Feed Me pour importer les nodes comme entries dans Craft. Il reste en général un peu de travail à effectuer manuellement mais la plupart des données peuvent être importées automatiquement.

## Exercices

### A faire ensemble

Construire un site d'agence simple à partir des templates reçus.

1. Homepage

- single section
- affiche les 3 derniers case studies (channel section)

2. Case studies (archive)

- liste paginée avec 12 case studies sur chaque page
- les case studies peuvent être filtrés par categorie

2. Case study (page de détail)

- chaque case study utilise 3 Matrix blocks (video, image, text) pour permettre la création de contenus modulaires

4. About page

- single section
- lists team members (channel section)
- lists values (structure section)

### A faire seul

1. Créer la section team, les custom fields et afficher les entries sur la page about (classées par nom de famille).
2. En plus des videos Youtube, ajouter la possibilité pour le champs Matrix des case studies d'afficher des videos provenant de Vimeo.
3. Ajouter une bannière responsive à la page about.

## Ressources

- [Documentation officielle](https://docs.craftcms.com/) de Craft
- [Tutoriel officiel](https://craftcms.com/docs/getting-started-tutorial/) pour bien commencer
- [Documentation Twig](http://twig.sensiolabs.org/doc/templates.html) pour les template designers
- [Craft stackexchange site](http://craftcms.stackexchange.com/): poser vos questions. L'équipe de Pixel&Tonic est présente.
- [Communauté Discord](https://craftcms.com/discord): poser vos question, aider les autres, chatter, etc.
