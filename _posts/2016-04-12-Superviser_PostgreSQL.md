---
date : 2016-04-12 22:52:14
layout: post
title: Superviser PostgreSQL 
category: french
tags: [PostgreSQL Supervision]

---

Il y a quelques jours j'ai participé au [PG Day Paris 2016](http://www.pgday.paris) en présentant un rapide tour d'horizon des solutions de supervision pour PostgreSQL. L'occasion de présenter l'état de l'art de l'écosystème Postgres en matière d'outil de visualisation !

<!-- MORE -->

![](  https://raw.githubusercontent.com/daamien/blog/gh-pages/_images/20160331_pgdayparis_superviser_postgresql.jpg  )

Les slides de ma présentation sont disponibles ci-dessous :

* Version HTML (avec videos): 
[http://dalibo.github.io/presentations/slides/fr/20160331-supervision/prez.md.html]()

* Version PDF  (sans videos): 
[http://dalibo.github.io/presentations/slides/fr/20160331-supervision/prez.beamer.pdf]()

Ma présentation s'est plutot bien déroulée et si j'en juge par les retours que j'ai eu le sujet intéresse beaucoup de mondes.

Plusieurs personnes sont venues me voir ensuite avec des remarques... Je profite de cet article pour y répondre globalement :


**1- C'était trop court !**

Oui 25 minutes pour parler d'une cinquantaine de logiciels, c'est forcément un peu lapidaire :) En même temps, ce format me convenait parfaitement car le but de la présentation était de donner des pistes et surtout une méthodologie pour évaluer les différentes solutions qui s'offrent aux DBA PostgreSQL. 

**2- Pourquoi est-ce que vous n'avez pas cité ELK parmi les solutions d'analyse de logs ?***

Il s'agit d'un demi-oubli.... Certes le trio ElasticSearch + Logstash + Kibana est très prometteur mais c'est aussi un pile applicative assez complexe à installer et à maintenir. Ma présentation était destiné à des DBA et ne traitait pas du monitoring en général. Et j'ai la convinction que si l'objectif est d'analyser uniquement les logs de PostgreSQL alors une solution comme pgBadger est 100 plus simple et efficiante que de monter un pile ELK from scratch. A contrario, si vous êtes DBA et qu'une chaine ELK est déjà en place, alors il peut être intéressant d'y injecter vos logs PostgreSQL. Voici un bon point de départ si le sujet vous intéresse : [](http://blog.2ndquadrant.com/redislog-integrating-postgresql-with-logstash-for-devops-real-time-monitoring/) 

**3- Pourquoi est-ce que vous n'avez pas parlé de l'impact négatif du monitoring sur les perfs ?**

Tout système de supervision fera pression sur votre instance PostgreSQL et impactera ses performances. Je n'ai pas abordé ce sujet volontairement car je déteste présenter des résultats de benchmarks dans une conférence. Afficher des chiffres et des graphes pendant quelques secondes sans pouvoir donner tous les éléments de contexte est totalement contre-prodcutif et on court toujours le risque que les auditeurs prennent des résultats ponctuels pour de règles absolues. Pour moi, le message important ce n'est pas de dire "le logiciel X aura moins d'impact négatif que le logiciel Y" , mais au contraire "Faites vos propres benchmarks et sélectionner l'outil le plus adapté à votre trafic et à votre métier. Pour savoir comment réaliser un benchmarch, vous pouvez consulter le récit du match [PoWA vs The Badger](https://github.com/dalibo/powa/wiki/POWA-vs-pgBadger) réalisé en 2014.


Au final, le PG Day Paris était une réussite et je remercie tous les organisateurs pour cet événement. 

Prochain Rendez-vous : le [PG Day France](http://www.pgday.fr) à Lille le 31 mai! Cette fois j'y serai en tant qu'organisateur.



