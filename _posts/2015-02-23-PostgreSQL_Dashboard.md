---
date: 2015-02-23 21:27:36 
layout: post
title: "PostgreSQL Dashboard"
description: "Un écran de suivi temps-réel basé sur Dashing et Sinatra"
category: french 
tags: [PostgreSQL,Dashboard,Monitoring,Dashing]
---

## Un écran de suivi temps-réel basé sur Dashing et Sinatra

PostgreSQL Dashboard est outil de supervision très simple qui donne une vue instantanée de la santé d'une instance PostgreSQL.


<!-- More -->

![](https://raw.githubusercontent.com/daamien/pgDashboard/master/public/pgdashboard.screenshot2.png )


L'outil est conçu pour être affiché sur un grand écran dans un salle de contrôle ou dans un espace de travail partagé. 

Le tableau de bord actuel est composé de 5 "widgets" :

* General Info : Version, nombre de bases, etc.
* Hit Ratio : Le % de données trouvées en cache
* Buffers : Le nombre de nouveaux buffers alloués
* Queries : Le nombre de requête en cours sur l'instance
* Twitter : Un aperçu du hashtag #PostgreSQL 


## Extensibilité

Ajouter un widget supplémentaire est très simple : cet outil est conçu pour faciliter l'écriture de widget "maison" et afficher des indicateurs métier. 

Par ailleurs, la mise en page est totalement flexible. Vous pouvez simplement faire un drag'n'drop sur chaque widget et le placer à l'endroit qui vous convient. Le code HTML du tableau de bord peut être modifié pour des besoins spécifiques comme par exemple les dimensions d'un écran. 

## Liens & Remerciements

Certaines parties de PostgreSQL Dashboard sont basées sur le travail effectués pour d'autres outil PostgreSQL, 
notamment [pgstats](https://github.com/gleu/pgstats) et [pgcluu](http://pgcluu.darold.net/)

PostgreSQL Dashboard est un projet ouvert distribué sous licence PostgreSQL. Toute contribution constructive est la bienvenue !
Vous pouvez envoyer vos idées, vos demandes et vos patches via les outils de GitHub.

**Pour aller plus loin :**

* [site web](http://daamien.github.io/PostgreSQL-Dashboard/)
* [code](https://github.com/daamien/PostgreSQL-Dashboard)


