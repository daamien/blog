---
date: 2018-11-05 10:17:36
layout: post
title: "Masquez vos données avec PostgreSQL Anonymizer"
description: "Anonymisation et masquage dynamique pour Postgres"
category: french
tags: ["PostgreSQL","data","masking", "anonymization", "scrambling"]
---

J'ai pubié il y a quelques jours une extension nommée [PostgreSQL Anonymizer]
qui masque ou remplace les données à caractère personnelle ( personally
identifiable information ou PII en anglais) et les données sensibles en
général.

Le projet est open source et disponible ici:

<https://gitlab.com/daamien/postgresql_anonymizer>

[PostgreSQL Anonymizer]: https://gitlab.com/daamien/postgresql_anonymizer

<!--MORE-->

![PostgreSQL Anonymizer](https://raw.githubusercontent.com/daamien/blog/gh-pages/assets/img/postgresql_anonymizer.banner.gif)

Je crois fermement dans une **approche déclarative** de l'anonymization :
le répérage des informations sensibles et les règles de masquage de ces données
devraient être décrites directement avec le langage SQL.

A l'age du RGDP, les développeurs devraient spécifier les stratégies d'anonymization
à l'intérieur même de la définition des tables, tout comme ils spéficient déjà
les types de données, les clés étrangères et les contraintes.

The projet est un **prototype** conçu pour démontrer la puissance d'une implemention
du masquage directement à l'intérieur de PostgreSQL. A l'heure actuelle, il est basé
sur la commande `COMMENT` (probablement la syntaxe la moins utilisée de PostgreSQL)
et un trigger sur les événement DDL.

Dans un futur proche, j'aimerai proposer une nouvelle syntaxe générale pour le
masquage dynamique. ([MS SQL Server possède déjà quelque chose de similiaire])

[MS SQL Server possède déjà quelque chose de similiaire]: https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking?view=sql-server-2017

L'extension peut être utilisée pour mettre un masque sur certains utilisateurs
ou pour modifier les données sensible de manière permanente. Plusieurs
méthodes sont possibles : la randomisation, le brouillage partiel ou encore les
règles ad-hoc.

__Voici un exemple simple et basique __ :

Imaginons une table `people`

```sql
=# SELECT * FROM people;
  id  |      name      |   phone
------+----------------+------------
 T800 | Schwarzenegger | 0609110911
```

Etape 1. Activer le moteur de masquage

```sql
=# CREATE EXTENSION IF NOT EXISTS anon CASCADE;
=# SELECT anon.mask_init();
```

Etape 2. Declarer un utilisateur masqué

```sql
=# CREATE ROLE skynet;
=# COMMENT ON ROLE skynet IS 'MASKED';
```

Step 3. Declarer les règles de masquage

```sql
=# COMMENT ON COLUMN people.name IS 'MASKED WITH FUNCTION anon.random_last_name()';

=# COMMENT ON COLUMN people.phone IS 'MASKED WITH FUNCTION anon.partial(phone,2,$$******$$,2)';
```

Step 4. Lire les données avec l'utilisateur masqué

```sql
=# \! psql test -U skynet -c 'SELECT * FROM people;'
  id  |   name   |   phone
------+----------+------------
 T800 | Nunziata | 06******11
```


Bien sur ce projet est encore en chantier. J'ai besoin de vos retours 
et de vos idées ! Dites-moi ce que vous pensez de cet outil, comment
il répond à vos besoins et quelles sont les fonctions qui vous manquent.

Vous pouvez soit [ouvrir un ticket] ou m'envoyer un message à <daamien@gmail.com>.

[ouvrir un ticket]: https://gitlab.com/daamien/postgresql_anonymizer/issues
