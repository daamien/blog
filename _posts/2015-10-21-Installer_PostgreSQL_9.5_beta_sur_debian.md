---
date : 2015-10-21 10:52:14
layout: post
title: Installer PostgreSQL 9.5 beta sur debian
category: french
tags: [PostgreSQL, debian, beta]
---

La [première version beta de PostgreSQL 9.5](http://blog.dalibo.com/2015/10/08/Sortie_de_PostgreSQL_9.5_beta.html) est sortie la semaine dernière. Voici un tuto rapide pour installer facilement cette version sur debian


<!--MORE-->

Première étape : Ajouter les dépots "PGDG", c'est à dire les dépots officiels de la communauté PostgreSQL. Pour cela il suffit de créer un fichier ``/etc/apt/sources.list.d/pgdg.list`` et d'y inscrire la ligne ci-dessous :

```
  deb http://apt.postgresql.org/pub/repos/apt/ jessie-pgdg main 9.5
```

Le mot clé ``main`` permet d'avoir accès à toutes les versions stables de PostgreSQL (de 9.0 à 9.4) et le mot clé  ``9.5`` est nécessaire pour avoir accès à la version beta. J'ai réalisé cette installation sur un serveur Jessie. Evidement si vous êtes sur une autre version debian, remplacez jessie par wheezie ou sid...

Ensuite on récupère la clé du dépot et la liste des paquets :

```bash
  sudo apt-get install wget ca-certificates
  wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
  sudo apt-get update
  sudo apt-get upgrade
```

Et voilà !

Il ne reste plus qu'à installer la version 9.5 :

```
  sudo apt-get install postgresql-9.5
```
