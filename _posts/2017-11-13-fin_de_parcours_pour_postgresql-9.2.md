---
date : 2017-11-13 09:52:14
layout: post
title: "Fin de parcours pour PostgreSQL 9.2"
category: french
tags: [PostgreSQL]

---

[PostgreSQL 10 est sortie il y a quelques
semaines](http://blog.dalibo.com/2017/10/05/Sortie_PostgreSQL_version_10.html) 
et un [premier correctif de
sécurité a été publié le 10
novembre](http://blog.dalibo.com/2017/11/10/sortie_PostgreSQL_10.1_9.6.6_9.5.10_9.4.1_9.3.20_9.2.24.html). 

Comme chaque année, la sortie d'une nouvelle version s'accompagne de la fin du  
support d'une version précédente. En l'occurence, c'est PostgreSQL 9.2, sortie  
en 2012, qui est désormais cataloguée comme "End Of Life" (EOL). 
                                                                   
<!--MORE-->                                                                     

Pour les nostalgiques, la version 9.2 était une étape importante puisqu'elle a 
marqué l'arrivée du type JSON et de la réplication Hot Standby en cascade. 5 ans
plus tard, PostgreSQL 10 complète le Hot Standby avec la 
[réplication logique](https://blog.anayrat.info/2017/07/29/postgresql-10-et-la-r%C3%A9plication-logique--fonctionnement/)
, et
fait jeu égal avec MongoDB avec des fonctionnalités JSON pleinement intégrées : 
indexation, procédures stockées PL/V8, recherche plein texte 

Tout ça pour vous dire que si vous avez des instances PostgreSQL 9.2 
(ou antiérieures) en production, il est temps de prévoir une montée de version 
dès que possible... Vous ne serez pas déçus !

  

