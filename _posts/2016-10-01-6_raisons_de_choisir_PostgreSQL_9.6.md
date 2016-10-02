---
date: 2016-10-03 11:17:36 
layout: post
title: "6 raisons de choisir PostgreSQL 9.6"
description: ""
category: français
tags: [PostgreSQL,parallelisme, parallelisation]
---

PostgreSQL 9.6 est sortie il y a quelques jours et encore fois c'est une version majeure
avec des fonctionnalités très attendues ! Les notes de versions sont très longues alors j'ai 
décidé de sélectionner 6 nouveautés importantes qui peuvent changer la manière dont vous utiliser 
PostgreSQL.


<!-- More -->

  ![](  https://c1.staticflickr.com/5/4002/4280292398_c5ca8c7176_z.jpg  )

1. **Le "Parallelisme"** est probablement l'attraction principale de cette version : 
une fonctionnalité réclamée depuis des années. Pour résumer : avec les versions
précédentes, Postgres utilisait uniquement un seul cœur par requête, même
d'autres processeurs étaient disponibles. Cette limititation vient d'être 
supprimée et différentes formes d'opérations peuvent désormais être effectuée 
en parallèle : parcours séquentiels, jointures et agregats peuvent être distribuée
sur plusieurs cœurs, si nécessaire.


2. **Un meilleur suivi des verrous** : la vue  ``pg_stat_activity`` fournit 
désormais des infos plus précises sur les blocages ("wait events"). Lorsqu'un
processus est en attente sur un verrou, vous pouver voir le type du verrous et 
des détails sur le contexte du blocage.  Par ailleurs, avec la fonction 
``pg_blocking_pids()`` vous pouvez voir ce qui bloque un processus donné. 
Ces outils de monitoring vont permettre aux DBA de savoir combien de temps un
processus a attendu un certain événement et donc mieux identifier les goulots 
d'étranglement. 

3. **La réplication synchrone multi-noeuds** vous garantit qu'une transaction 
est appliquées simultanément sur plusieurs instances. Cela permet de renforcer
la durabilité des données et en dupliquent les données sur plusieurs instances
parfaitement identiques.


4. **Moins d'espace perdu !** Jusqu'à présent les requêtes très longues ou les
curseurs pouvait empêcher le nettoyage des lignes mortes et provoquer un 
gonflement ("bloat") des volumes de stockage, ce qui impactait les performances et 
causait une surconsommation d'espace disque.  La nouvelle option 
``old_snapshot_threshold`` autorise PostgreSQL à nettoyer un ligne morte lorsque
 la transaction qui l'a modifiée est terminée et que les snapshots qui peuvent
encore la voir ont dépassé un certain age.

5. **Des améliorations sur les Data Wrappers** : Avec plus de 80 connecteurs 
externes (FDW) vous pouvez déjà brancher PostgreSQL sur n'importe quel système
de stckage de données. La version 9.6 apporte des nouveautés importantes sur le 
connecteur postgres_fdw, notamment le transfert ( "push down") des jointures et 
des tris sur le nœud distant. Une avancée fondamentale pour agréger des données
à partir de plusieurs instances PostgreSQL.  

6. **Le mode remote_apply** : PostgreSQL 9.6 propose une nouvelle méthode de 
réplication dans laquelle le nœud primaire attend que les changements soient 
appliqués sur le nœeud secondaire. C'est un mode un peu plus lent que la 
réplication classique mais cela  garantit que les "données commitées" sur un 
nœud secondaire sont bien visibles. Idéal pour distribuer les lectures sur 
plusieurs instances !

Bien sûr retenir uniquement 6 items dans la liste des nouveautés de Postgres 
9.6 est un choix difficile. Il y a beaucoup d'autres améliorations dans cette 
version, par exemple : le rechercher plein texte sur des phrases entières, 
l'extension pg_visibility, un meilleur nettoyage (VACUUM) des pages gelées, les
parcours unique d'index pour les index partiels, le suivi de la progression
des commandes, et comme d'habitude.... une flopée de gains de performance !


Pour un tour d'horizon plus détaillé, la page de wiki [What's New in 9.6](https://wiki.postgresql.org/wiki/NewIn96)
est un bon point de départ !



``Photo Credit :`` [duncan @ Flickr](https://www.flickr.com/photos/duncan/4280292398/sizes/z/) CC BY-NC
