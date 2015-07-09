---
date : 2015-07-09 10:14:14
layout: post
title: PostgreSQL 9.5 Alpha 1 
category: french
tags: [PostgreSQL]
---

Dans la chaleur de la semaine dernière, la première mouture de PostgreSQL 9.5 est sortie en toute discrétion. Pourtant cette première version alpha apporte une série de nouveautés impressionnantes !

Voici un petit tour d'horizon de ce qui vous attend...

<!--MORE-->


Il est impossible de dresser une liste exhaustive des nouveautés et la sélection ci-dessous est tout à fait subjective :

* La commande [IMPORT FOREIGN SCHEMA](http://www.depesz.com/2014/07/14/waiting-for-9-5-implement-import-foreign-schema/) va complètement changer la donne en ssimplifiant l'intégration de données (voir mon article sur les [70 nuances Postgres](http://blog.taadeem.net/french/2015/03/16/70_nuances_de_Postgres/)

* Les [Index Block-Range](https://wiki.postgresql.org/wiki/What%27s_new_in_PostgreSQL_9.5#BRIN_Indexes) (BRIN) proposent une nouvelle méthode d'indexation ultra-compacte très utile pour les gros volumes de données 

* La sécurité au niveaux des lignes ( ''Row Level Security'' ) permet désromais de définir des politiques d'accès en fonction du contenu d'une table. 

* La commande [INSERT ON CONFLICT UPDATE](https://www.youtube.com/watch?v=pbg97bkxbbY) plus connue sous le nom ''UPSERT'' était une fonctionnalité très attendue

* Il est maintenant possible d'utiliser l'[héritage avec les tables étrangères](http://michael.otacoo.com/postgresql-2/postgres-9-5-feature-foreign-table-inheritance/). En clair il est désormais possible d'exporter une table partitionnée sur une instance distante et ainsi externaliser la croissance de vos bases ("scale-out")

* Une nuée de nouvelles fonctions [JSONB](https://wiki.postgresql.org/wiki/What%27s_new_in_PostgreSQL_9.5#JSONB-modifying_operators_and_functions)


Pour une liste plus détaillée, rendez-vous sur la page [What's new in PostgreSQL 9.5](https://wiki.postgresql.org/wiki/What%27s_new_in_PostgreSQL_9.5)


## A vous de jouer !

Si vous aimez PostgreSQL mais que vous ne savez pas comment contriber, voici une belle occasion !

Les développeurs de PostgreSQL ont suspendu l'intégration de nouvelles fonctionnalités dans PostgreSQL et ils vont consacrer les prochaine semaines à la stabilisation du code. 

Pour les aider, il suffit d'installer, de compiler et de tester PostgreSQL 9.5 dans avec vos applications et répondez aux questions suivantes :

* Constatez-vous des améliorations de performances ?
* Est-ce les changements effectués provoquent des erreurs sur vos plate-forme ?
* Le format des journaux de transation a changé (compression). Est-ce que vos système de failover et de PRA fonctionnent comme prévu ?
* Est-ce que la sécurité au niveu ligne (RLS) protège vos données correctement ?

La qualité des tests est essentielle dans le processus de développement de PostgreSQL. Des paquets sont disponibles pour la plupart des distributions et une image Docker a été préparée pour simplifier les tests. (voir lien ci-dessous) 

## Objectif fin 2015

Cette première version est désignée comme "alpha" car des changements importants sont encore possible. La première version est "beta" est prévue pour le mois d'aout et la version stable est attendue pour la fin 2015.


## Liens 

  * [Page de Téléchargement](http://www.postgresql.org/download/)
  * [Image Docker](https://wiki.postgresql.org/wiki/PostgreSQLTestDockerImage)
  * [Comment tester](https://wiki.postgresql.org/wiki/HowToBetaTest)
