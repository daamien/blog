---
date: 2017-10-27 08:17:36 
layout: post
title: "Publication de notre workshop : Les nouveautés de PostgreSQL 10"
description: "Découvrez PostgreSQL 10 avec un manuel d'exercices complet"
category: french
tags: [PostgreSQL]
---

Pour chaque nouvelle version de PostgreSQL, Dalibo organise des ateliers d'une
journée pour présenter les dernières nouveautés et les améliorations. Les places 
étant limitées, nous avons décidé de diffuser plus largement
le contenu de nos workshops en publiant les fichiers source sous licence libre
via github.

<!-- More -->

[![PostgreSQL 10
Workshop](_images/cover_workshop_10.png)](https://cloud.dalibo.com/p/exports/formation/workshops/fr/100-postgresql_10.pdf)

Vous pouvez retrouver nos deux derniers workshops sur ce dépot :

<https://github.com/dalibo/workshops>

Chaque workshop comprend une cinquantaine de slides et un manuel avec des
explications détaillées et des exercices.

Le contenu est écrit en  markdown en utilisant un ensemble spécifique de
[règles de syntaxe](https://github.com/dalibo/workshops/blob/master/SYNTAX.md). 
On utilise ensuite [pandoc](http://pandoc.org/) pour exporter le contenu sous 
différent formats. Pour l'instant les formats supportés sont : reveal (HTML), 
PDF, EPUB, doc.



Installation et Compilation
--------------------------------------------------------------------------------------------

Vous pouvez compiler ces documents vous-mêmes en utilisant a une [image docker
prévue à cet effet](https://github.com/dalibo/workshops/blob/master/QUICKSTART.md) 
ou [installer la chaine de production complète](https://github.com/dalibo/workshops/blob/master/INSTALL.md) 
en local (basée sur debian). 


Ensuite la compilation est assez basique. Chaque workshop est écrit dans un
fichier markdown unique (par exemple
[fr/100-postgresql_10.md](https://github.com/dalibo/workshops/blob/master/fr/100-postgresql_10.md)
)

Et vous exporter le contenu avec make en spécifiant l'extension que vous
voulez:

```
make fr/100-postgresql_10.html
make fr/100-postgresql_10.epub
make fr/100-postgresql_10.pdf
etc.
```

Vous pouvez aussi compiler tous les  workshops dans tous les formats avec:

```
make all
```

Et pour les paresseux, on a mis aussi les versions compilés ici:

<https://github.com/dalibo/workshops/blob/master/fr/README.md>

;-)




Vos Contributions sont les bienvenues !
-------------------------------------------------------------------------------

Ce projet est ouvert à toutes les bonnes volontés. Vous pouvez contribuer de
différentes façons :

* déclarer un bug
* corriger une typo
* traduire
* proposer une nouveau workshop
* etc.

Plus de détails :
<https://github.com/dalibo/workshops/blob/master/CONTRIBUTING.md>

