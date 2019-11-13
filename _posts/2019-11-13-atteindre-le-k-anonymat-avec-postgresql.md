---
date: 2019-11-13 11:17:36
layout: post
title: "Atteindre le k-anonymat avec PostgreSQL"
description: "Généralisation de données avec les types RANGE"
category: french
tags: ["PostgreSQL","RGPD","masquage", "anonymisation", "generalisation"]
---

[Dalibo Labs] vient de publier la version [version 0.5] de 
[PostgreSQL Anonymizer], une extension qui masque les données sensibles
à l'intérieur d'une base de données PostgreSQL.

[Dalibo Labs]: https://labs.dalibo.com
[version 0.5]:  https://www.postgresql.org/about/news/1993/
[PostgreSQL Anonymizer]: https://gitlab.com/dalibo/postgresql_anonymizer/

<!--MORE-->

L'extension disposait déjà jusqu'ici d'un large panel de méthodes 
d'anonymisation : [randomisation], [imitation], [destruction partielle], 
[brassage], [bruit], etc.

Pour cette nouvelle version, j'ai travaillé sur une nouvelle approche appelée [Géneralisation] et le concept de [k-anonymat].

[randomisation]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#randomization
[imitation]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#faking
[destruction partielle]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#partial-scrambling
[brassage]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#shuffling
[bruit]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#adding-noise
[Généralisation]: https://postgresql-anonymizer.readthedocs.io/en/latest/generalization/
[k-anonymat]: https://postgresql-anonymizer.readthedocs.io/en/latest/generalization/#k-anonymity

## La Généralisation, un principe basique et répandu

L'idée de la [Généralisation] est toute simple: pour éviter de donner une 
information sensible, on peut la remplacer par une valeur moins précise.

Par exemple, au lieu de dire "Bob a 28 ans", on peut déclarer "Bob a entre 20 
et 30 ans". C'est quelque chose que nous faisons tous inconsciemment au 
quotidien lorsque nous ne voulons pas révéleer une information précise. 
Typiquement quand quelqu'un demande 'Où habitez-vous ?", le plus souvent 
nous répond de manière vague ( "J'habite à Paris" ou "J'habite dans le sud de la France") au lieu de donner notre adresse complète. 

Ce qui est intéressant avec cette approche, c'est que les **données anonymisées restent vraies**.

La [Généralisation] est donc une manière simple et intuitive de protéger son anonymat pour les êtres humains. Mais comment appliquer cela à un système de 
gestion de base de données ? Après tout l'objectif principal d'une base de 
données comme PostgreSQL est de stocker des données précises. Si vous avez 
une colonne `date_de_naissance` dans une table, vous ne pouvez pas insérer 
la valeur "dans les années 90"... 😀

Et bien il se trouve que PostgreSQL peut gérer la généralisation très 
facilement avec les types [RANGE], un outil puissant pour stocker et 
manipuler des plages de données bornées par une limite basse et une 
limite haute. 

[RANGE]: https://www.postgresql.org/docs/current/rangetypes.html


Exemple: Généralisation de données médicales
--------------------------------------------------------------------------------

Voici une table basique contenant des données de santé:

```sql
# SELECT * FROM patient;
     ssn     | firstname | zipcode |   birth    |    disease    
-------------+-----------+---------+------------+---------------
 253-51-6170 | Alice     |   47012 | 1989-12-29 | Heart Disease
 091-20-0543 | Bob       |   42678 | 1979-03-22 | Allergy
 565-94-1926 | Caroline  |   42678 | 1971-07-22 | Heart Disease
 510-56-7882 | Eleanor   |   47909 | 1989-12-15 | Acne
 098-24-5548 | David     |   47905 | 1997-03-04 | Flu
 118-49-5228 | Jean      |   47511 | 1993-09-14 | Flu
 263-50-7396 | Tim       |   47900 | 1981-02-25 | Heart Disease
 109-99-6362 | Bernard   |   47168 | 1992-01-03 | Asthma
 287-17-2794 | Sophie    |   42020 | 1972-07-14 | Asthma
 409-28-2014 | Arnold    |   47000 | 1999-11-20 | Diabetes
(10 rows)
```

Nous voulons que les données anonymisées restent vraies car elles vont être 
utilisées pour calculer des statistiques. Nous allons donc construire une vue 
matérialisée au-dessus de cette table avec 2 actions :

1. D'abord, nous devons retirer les clés primaires et les identifiants 
   directs. Dans ce cas, nous allons simplement supprimer le champs `ssn`
   et nous allons détruire les valeurs du champs `firstname`.  

2. Ensuite, nous allons réduire la précision des [identifiants indirects] 
   ( `zipcode` and `birth`) avec 2 [fonctions de généralisation] fournies    
   par l'extension  [PostgreSQL Anonymizer] : `anon.generalize_int4range` et 
   `anon.generalize_daterange`.

C'est parti: 

```sql
CREATE MATERIALIZED VIEW generalized_patient AS
SELECT
    'REDACTED'::TEXT AS firstname,
    anon.generalize_int4range(zipcode,1000) AS zipcode,
    anon.generalize_daterange(birth,'decade') AS birth,
    disease
FROM patient;
```

Notez que les [fonctions de generalisation] prennent 2 arguments: 
la valeur originale en première option et un deuxième paramètre 
pour décrire la taille des "paliers" du type [RANGE].

[fonctions généralisation]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#generalization

Pour les valeurs numeriques, le second paramètre est un entier. Ici le code postal (la colonne `zipcode`) est généralisé par palier de 1000. 

Pour les valeurs temporelles, les paliers possibles sont : microseconds, milliseconds, second, minute, hour, day, week, month,
year, decade, century and millennium. Ici la colonne `birth` est 
généralisée par tranches de 10 ans.

Ce qui nous donne une "vue vague" des données :

```sql
# SELECT * FROM generalized_patient;
 firstname |    zipcode    |          birth          |    disease    
-----------+---------------+-------------------------+---------------
 REDACTED  | [47000,48000) | [1980-01-01,1990-01-01) | Heart Disease
 REDACTED  | [42000,43000) | [1970-01-01,1980-01-01) | Allergy
 REDACTED  | [42000,43000) | [1970-01-01,1980-01-01) | Heart Disease
 REDACTED  | [47000,48000) | [1980-01-01,1990-01-01) | Acne
 REDACTED  | [47000,48000) | [1990-01-01,2000-01-01) | Flu
 REDACTED  | [47000,48000) | [1990-01-01,2000-01-01) | Flu
 REDACTED  | [47000,48000) | [1980-01-01,1990-01-01) | Heart Disease
 REDACTED  | [47000,48000) | [1990-01-01,2000-01-01) | Asthma
 REDACTED  | [42000,43000) | [1970-01-01,1980-01-01) | Asthma
 REDACTED  | [47000,48000) | [1990-01-01,2000-01-01) | Diabetes
(10 rows)
```

La beauté des types [RANGE] est qu'ils peuvent être facilement manipulés 
pour des traitements statistiques. Par exemple, on peut répondre à la 
question "Combien de personnes nées avant 1990 ont eu la grippe ?" 
avec la requête ci-dessous:

```sql
SELECT count(*)
FROM generalized_patient
WHERE disease = 'Flu'
AND upper(birth) < '1990-01-01';
```

Comme on peut le voir, plus on généralise avec des paliers très larges, 
moins les données seront précises. Par exemple, utiliser la valeur 
`millennium` dans la fonction  `anon.generalize_daterange()` va complètement 
détruire les données. A l'inversie, utiliser la valeur `week` sera trop
juste et il y aura un risque de réidentification.

La question corrolaire liée à la [Généralisation] est donc : "Est-ce que 
ce jeu de données est suffisament généralisé ?" et c'est ici que nous 
rencontrons le concept de [k-anonymat].

## k-anonymat ou comment évaluer le risque de ré-identification


[k-anonymat] est un terme utilisé pour décrire une propriété d'un ensemble 
de données anonymisées. Le principe de [k-anonymat] stipule qu'à l'intérieur 
d'un ensemble de données, tout individu anonymisé ne peut pas être distingué
d'au moins `k-1` autres individus. Autrement dit, le [k-anonymat] évalue le 
degré de redondance des informations identifiantes. Une valeur faible de 
de `k` indique qu'il y a un risque de ré-identification en utilisant 
notamment la technique du recoupement avec des jeux de données externes. 

Avec [PostgreSQL Anonymizer], on peut évaluer le facteur `k` en 2 étapes :

1. Tout d'abord déclarer les colonnes qui sont des [identifiants indirects]
   ( également appelés "quasi identifiants") comme ceci:
   
```sql
SECURITY LABEL FOR anon ON COLUMN patient.zipcode IS 'INDIRECT IDENTIFIER';
SECURITY LABEL FOR anon ON COLUMN patient.birth IS 'INDIRECT IDENTIFIER';
```

2. Une fois que les identifiants indirects sont déclaré, il suffit 
   d'utiliser la fonction `k_anonymity()`:

```sql
SELECT anon.k_anonymity('generalized_patient')
```

Dans l'exemple ci-dessus, le facteur `k` est 3, ce qui signifie que pour 
chaque valeur du couple (`zipcode`,`birth`), il y a au moins 3 individus 
dans la table `generalized_patient`. Lorsque `k = 1`, certaines patients 
peuvent être réidentifiés à partir de données externes ou par inférence. 

## Différentes techniques d'anonymisation pour différents usages

Vous avez probablement déjà compris qu'il y a 2 limitations liées à la 
méthode de [Généralisation]:


A/ Cette technique marche très bien pour les jeu de données ayant peu de
   dimensions (i.e les tables avec peu de colonnes). En revanche si un  
   ensemble de données contient beaucoup d' [identifiants indirects], le 
   la facteur `k` va chuter.
   
B/ La vue généralisée et la table originale ont des schémas de données 
   différents. Concrètement, les valeurs numériques et temporelles sont 
   remplacées par des [RANGE]. Puisque la [Généralisation] "casse" le 
   modèle de données, il n'est pas possible d'utiliser le 
   [Masquage Dynamique], les [exports anonymes] et la [substitution] 
   avec cette stratégie. Autrement dit: la [Généralisation] ne peut pas
   être utilisée pour les tests d'intégration continue, pour les   
   environnements de développement ou sur des instances de formation.
   
[exports anonymes]: https://postgresql-anonymizer.readthedocs.io/en/latest/anonymous_dumps/
[substitution]: https://postgresql-anonymizer.readthedocs.io/en/latest/in_place_anonymization/
[Masquage Dynamique]: https://postgresql-anonymizer.readthedocs.io/en/latest/dynamic_masking/
[identifiants indirects]: https://labkey.med.ualberta.ca/labkey/_webdav/REDCap%20Support/@wiki/identifiers/identifiers.html?listing=html

On a ici l'illustration qu'il n'y a pas d'approche unique de l'anonymisation. 
En fait, dans certains vous devrez probablement construire plusieurs flux
d'anonymisation pour la même base de données, car vos interlocuteurs auront
des besoins et des contraintes différentes. 

C'est tout pour aujourd'hui ! Si le RGPD et l'anonymisation sont un sujet 
important pour vous, jetez un oeil à l'extension [PostgreSQL Anonymizer] 
et **envoyez-nous du feedback** ! 

On adore le feedback 😀

Ce projet est open source et disponible à l'adresse ci-dessous: 

https://gitlab.com/dalibo/postgresql_anonymizer/
