---
date : 2016-06-30 09:52:14
layout: post
title: "Sortie de PostgreSQL 9.6 beta 2"
category: french
tags: [PostgreSQL]

---

Il y a une semaine sortait [la seconde beta de la future version de PostgreSQL](https://www.postgresql.org/about/news/1677/). 
De nombreux bugs ont été découverts et corrigés depuis la permière beta1, en particulier sur tout ce qui concerne le parallelisme. Si vous n'avez pas encore eu le temps de découvrir les nouveautés qui vous attendent c'est le moment ! 

Le menu est copieux :

* Traitement parallèle des requêtes (scans séquentiels, jointures et aggrégats)
* Pour le connecteur postgres_fdw, transmission (pushdown) des tris, des jointures et ordres DML (update/Delete) sur les tables externes
* Pour le réplication Hot Standby :  gestion de plusieurs instances synchrones et nouveau mode "remote_apply"
* Pour les transactions :  l'extension pg_visibility
* Support des opérateurs KNN pour l'extension Cube
* Un indicateur de progression des commandes
* Plus d'informations sur les temps d'attente ('waits') dans pg_stat_activity
* Parcours d'index seul (Index-only scans) pour les index partiels
* Recherche plein texte sur des phrases entières
* Nouvelle API pour les sauvegardes physiques

[Et plus encore !](https://wiki.postgresql.org/wiki/Newin96)

**Attention !** Il y a eu pas mal de changements notables ces dernières semaines. Vous devez migrer les données de la beta1 vers la beta2 en rechargeant les données (sauvegarde puis restauration) et vous obtiendrez peut-être des erreurs pendant ce processus. Les principaux changements sont :

* Le paramète ''max_parallel_degree'' est renomé ''max_parallel_workers_per_gather''
* une fonction de vérification d'intégrité a été ajouté à pg_visibility
* ajout de VACUUM (DISABLE_PAGE_SKIPPING) en cas d'urgence
* ajout de la fonction pg_truncate_visibility_map
* ajout du paramètre min_parallel_relation_size
* désactivation du paramètre backend_flush_after

Pour suivre les bugs détectés et l'avancée des correctifs, rendez-vous la page [Open Items](https://wiki.postgresql.org/wiki/PostgreSQL_9.6_Open_Items)

Et si vous cherchez un moyen de découvrir cette nouvelle version tout en contribuant à la communauté, il reste encore [quelques pages de la doc à traduire](http://blog.guillaume.lelarge.info/index.php/post/2016/05/20/Quelques-nouvelles-sur-les-traductions-du-manuel) !

