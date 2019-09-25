---
date: 2019-09-25 10:17:36
layout: post
title: Nouvelle version de PostgreSQL Anonymizer et plus...
description: "Les amendre RGPD arrivent !"
category: french 
tags: ["PostgreSQL","data","masking", "anonymization", "GDPR"]
---

Il y a un an, j'ai lancé un projet appelé [PostgreSQL Anonymizer] pour étudier 
et aprrendre différentes techniques de protection des données privées en 
utilisant la puissance de PostgreSQL.
Ce projet fait maintenant partie de l'initiative  [Dalibo Labs] et nous venons
de publier une [nouvelle version] la semaine dernière....

<!--MORE-->

C'est l'occasion d'analyser les progrès réalisés, l'impact du RGPD et les
futures directions du projet. 

[PostgreSQL Anonymizer]: https://gitlab.com/dalibo/postgresql_anonymizer
[Dalibo Labs]: https://labs.dalibo.com
[nouvelle version]: http://blog.dalibo.com/2019/09/13/postgresql_anonymizer_0.3_EN.html


![](https://raw.githubusercontent.com/dalibo/blog/gh-pages/img/PostgreSQL-Anonymizer_H_couleur.png)

## RPGD : les premières sanctions arrivent 

Depuis le début de ce projet, le paysage a radicalement changé.... Lorsque le 
RPGD est entré en vigueur en mai 2018, uns des grandes questions concerant
les pénalités financières infligées. Est-ce que les montants serait assez 
significatifs pour provoquer un réel changement dans la manière dont les 
entreprises gèrent les données personnelles ?

De ce que l'on peut voir, [les amendes RGPD ont commencé à tomber] depuis le 
début d'année et rien que pour le mois de juillet 2019 : 
Bristish Airways  a reçu une sanction de 204 M€ et les hotels Marriott 
Hotels en a reçu une de 110 M€.

Il y a aussi des amemdes moins élevée pour des sociétés de petites tailles. 
Ce qui est intéressant, c'est que les sanctions les plus importantes 
concernent l'[Article 32] et « des mesures techniques et organisationnelles 
insuffisante pour garantir le sécurity de l'information ».

En d'autres termes : des **fuites de données**.

Voici un domaine ou l'anonymisation peut aider ! Sur la base de mon expérience,
il est possible de réduire les risques de fuites d'informations sensibles en
limitant la dispersion de ces données. Dans beaucoup d'environnements ( 
pré-production, formation, développement, intégration, statistiques, etc...) 
les données réelles ne sont pas absolument nécessaires. Avec une politique
d'anonymisation très forte, on peut resteindre l'usage des données 
personnelles uniquement lorsque c'est indispensable et travailler sur des 
données aléatoires ou artificielles partout ou c'est possible... Lorsque
l'anonymisation est faite correctement, les jeux de données anonymisées ne 
sont plus soumises aux contraintes du RGPD.

Pour résumer: l'anonymisation est une méthode puissante pour réduire la 
**surface d'attaque** et c'est un clé pour limiter les risques  
liés aux fuites de données.

C'est pourquoi nous investissons beaucoup d'efforts pour développer des outils
de masquage directement à l'intérieur de PostgreSQL !


[Article 32]: http://www.privacy-regulation.eu/en/32.htm
[les amendes RGPD ont commencé à tomber]: http://www.enforcementtracker.com/

## Des améliorations majeures 

Le mois dernier, j'ai travaillé sur différents aspects de l'extension, 
notamment :

* Ajouter plus de tests et de protection contre les injections SQL
* Faciliter l'export avec les [Dumps Anonymes] et la superbe extension [ddlx]
* Ajouter plus de [Fonctions de Masquage], en particulier le `mélange` et 
  la `génération de bruit`.
* Réécrire de larges parties du [Moteur de Masquage]
* Clarifier les concepts d' [Anonymisation Permanente]
* Ecrire une meilleur [documentation] :-)



[ddlx]: https://github.com/lacanoid/pgddl
[documentation]: https://postgresql-anonymizer.readthedocs.io/
[Règles de Masquage]: https://postgresql-anonymizer.readthedocs.io/en/latest/declare_masking_rules/
[Fonctions de Masquage]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/
[Dumps Anonymes]: https://postgresql-anonymizer.readthedocs.io/en/latest/anonymous_dumps/
[Anonymisation Permanente]: https://postgresql-anonymizer.readthedocs.io/en/latest/in_place_anonymization/
[Masquage Dynamique]: https://postgresql-anonymizer.readthedocs.io/en/latest/dynamic_masking/




## Security Labels

Un défaut de l'implémentation actuel de PostgreSQL Anonymizer est que les 
[Règles de Masquage] sont déclarées avec la syntaxe `COMMENT`, ce 
qui peut être ennuyeux lorsque votre modèle de données contient déjà 
des commentaires. 

Grace à une idée d'Alvaro Herrera, je travaille actuellement sur une nouvelle
syntaxe basée sur les [Labels de Sécurité], une fonctionnalité méconnue 
qui est utilisée principalement par l'extension [sepgsql].


[sepgsql]: https://www.postgresql.org/docs/current/sepgsql.html

[Labels de Sécurité]: https://www.postgresql.org/docs/current/sql-security-label.html

```sql
SECURITY LABEL FOR anon ON COLUMN people.zipcode
IS 'MASKED WITH FUNCTION anon.fake_zipcode()'
```

Cette syntaxe devrait disponible dans quelques semaines. Bien sur la syntaxe
précédentes sera toujours supportées pour assurer la retro-compatibilité.


## Discutons !

Le RGPD et la protectée des données personnelles sont deux sujets brulants !
Je parlerai de tout cela dans différents événement cet automne, notamment:

* le 14 Octobre, à l'occasion de  [PostgreSQL Conference Europe] à Milan
* le 14 Novembre , à l'occasion du [Libday / Devops D-Day] à Marseille 

[PostgreSQL Conference Europe]: https://www.postgresql.eu/events/pgconfeu2019/schedule/session/2780-anonymization-gdpr-and-beyond/
[Libday / Devops D-Day]: https://2019.libday.fr/programme/

If you have any ideas or comments on PostgreSQL Anonymizer or more generally 
about protecting data privacy with progress, please send me a message at 
<damien@dalibo.com>.
