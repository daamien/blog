---
date: 2015-03-16 11:17:36 
layout: post
title: "70 Nuances de Postgres"
description: "PostgreSQL en tant que plateforme d'intégration de données"
category: french
tags: [PostgreSQL,FDW,ETL]
---

Le support du standard SQL/MED a été introduit dans PostgreSQL 9.1 (2011). Quatre ans plus tard, nous avons maintenant plus de [70 Foreign Data Wrappers](https://wiki.postgresql.org/wiki/FDW?nocache=1) (``FDW``) disponibles pour lire et écrire sur tout type de moteur de stockage: base Oracle ou MongoDB, fichiers XML, cluster Hadoop, dépot Git, Twitter, etc. etc. etc.

<!-- More -->

![](https://raw.githubusercontent.com/daamien/blog/gh-pages/_images/catwrap.jpg)

Il y a quelque jours, [pendant le FOSDEM](http://blog.taadeem.net/english/2015/01/14/fosdem_2015http://blog.taadeem.net/english/2015/01/14/fosdem_2015) j'ai assisté à la conférence de mon collègue [Ronan Dunklau](http://rdunklau.github.io/about/) intitulée [Foreign Data Wrappers in PostgreSQL : Where are we now ?](https://fosdem.org/2015/schedule/event/foreign_data_wrappers_in_postgresql_where_are_we_now/) Ronan est l'auteur de l'extension [multicorn](http://multicorn.org/) et pendant sa présentation je me suis rappelé à quel point multicorn est probablement le projet le plus sous-estimé de la communauté PostgreSQL. En guise d'exercice, j'ai commencé à compter tous les FDW que je connaissais... et j'ai rapidement réalisé qu'il y en avait trop pour ma mémoire de poisson rouge !

Du coup, je suis retourné sur la  [lise de FDW du wiki PostgreSQL](https://wiki.postgresql.org/wiki/FDW?nocache=1). J'ai commencé à mettre à jour et reformatter le catalogue de tous les wrappers existants et j'ai abouti à une liste de plus de 70 wrappers pour PostgreSQL... 


## Quelques leçons à en tirer

Pendant cet inventaire, j'ai découvert quelques points intéressants:

* Tous les [SGBD majeurs](https://wiki.postgresql.org/wiki/FDW#Specific_SQL_Database_Wrappers) sont couverts à l'exception de DB2. Bizarrement DB2 est aussi le seul SGBD majeur ayant une implémentation du standard SQL/MED. Difficile de dire si ces deux faits sont liés ou pas... 


* Un tiers des FDW sont écrits en Python et basés sur Multicorn. Les autres sont des wrappers "natifs" écrits en C. Assez logiquement les wrappers en C sont prvilégiés pour les [connecteurs de SGBD](https://wiki.postgresql.org/wiki/FDW#Specific_SQL_Database_Wrappers) (ODBC, Oracle, etc.) pour des raisons de performance. En parallèle Multicorn est utilisé majoritairement pour les [interroger des web services](https://wiki.postgresql.org/wiki/FDW#Generic_Web_Wrappers) (S3 storage,RSS files, etc.) et des [formats de données specifiques](https://wiki.postgresql.org/wiki/FDW#Scientific_Wrappers) (fichiers de genotype, GeoJSON, etc.) 

* Je ne pense pas que c'était le but initial mais les Foreign Data Wrappers sont aussi un vecteur d'innovation. Certains connecteurs sont détournés de leur usage
premier pour développer de nouvelles fonctionnalités. Par exemple, CitusDB a publié une [base de données orientée colonnes](https://wiki.postgresql.org/wiki/FDW#Column-Oriented_Wrappers) basée sur un FDW, d'autres wrappers vont dans des [directions encore plus exotiques](https://wiki.postgresql.org/wiki/FDW#Exotic_Wrappers) comme la collecte de stats au niveau OS, de l'accéleration GPU pour les seq scans ou un moteur traitement parallèle de requêtes... La plupart de ces projets ne conviennent pas pour de la production bien sur. Cependant cela démontre qu'il est aisé d'implémenter un PoC et à quel point cette technologie est versatile. 


* Le [réseau de distribution PGXN](http://pgxn.org/) semble plutot méconnu. Seulement 20% des wrappers sont disponibles via PGXN. Ou peut-être que les développeurs sont trop paresseux pour créer un paquet pour leur extension ?

* Le succès de l'implémentation d' SQL/MED dans PostgreSQL a toutefois des inconvénients. A vue de nez, il y aura plus de 100 wrappers disponibles à la fin 2015. Pour certaines sources de données comme mongodb ou LDAP, il y a déjà plusieurs wrappers différents, sans parler de la cohorte de [connecteurs Hadoop](https://wiki.postgresql.org/wiki/FDW#Big_Data_Wrappers) :) Sur le long terme, les utilisateurs de PostgreSQL auront du mal à déterminer quels sont les wrappers maintenus et lesquels sont abandonnés... La page de wiki est une tentative pour répondre à cette question mais il y a surement d'autres solutions pour fournir des informations précises et exhaustives aux utilisateurs. Peut-être qu'il faudrait un data Wrapper qui pourrait lister tous les data wrappers  :) 


## The next big thing : IMPORT FOREIGN SCHEMA 

De plus, il reste une limitation majeure à l'implémentation actuelle : les **meta données**. Actuellement, il n'y a pas de moyen simple d'utiliser les (éventuelles) capacités d'introspection de la source de données distante. En clair : vous devez créer toutes vos tables externes une par une. Lorsque l'on veut se connecter à toutes les tables d'une base distantes, c'est répétitif et il y a un risque d'erreurs. Si vous vous connectez à une base PostgreSQL distante, vous pouvez utiliser des [astuces](https://news.ycombinator.com/item?id=8027472) mais clairement c'est une tache qui devrait être faite au niveau du connecteur lui-même.

Heureusement voici la commande ``IMPORT FOREIGN SCHEMA`` ! Cette nouvelle fonctionnalité a écrite par Ronan Dunklau, Michael Paquier et Tom Lane. Vous pouvez lire une démo rapide sur le [blog de Michael](http://michael.otacoo.com/postgresql-2/postgres-9-5-feature-highlight-import-foreign-schema/). Cette commande sera disponible dans la version 9.5.

C'est une amélioration énorme ! Avec cet ``IMPORT`` et les douzaines de wrappers disponibles, PostgreSQL est en passe de devenir une plateforme d'intégration de données unique et le besoin d'utiliser des outils d'ETL externes devient de plus en plus faible. 

![PostgreSQL as a Data Integration Plateform](https://raw.githubusercontent.com/daamien/blog/gh-pages/_images/800px-Dataintegration.png)

**Liens :**

* [l'inventaire des FDW sur le wiki](https://wiki.postgresql.org/wiki/FDW?nocache=1)
* [multicorn](http://multicorn.org/)
* [IMPORT FOREIGN SCHEMA](http://www.postgresql.org/docs/devel/static/sql-importforeignschema.html)

EDIT : Merci à Eric Pommereau pour la relecture !
