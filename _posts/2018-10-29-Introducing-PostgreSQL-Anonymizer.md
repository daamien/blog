---
date: 2018-10-29 10:17:36
layout: post
title: Introducing PostgreSQL Anonymizer
description: "Anonymization & Data Masking for Postgres"
category: english
tags: ["PostgreSQL","data","masking", "anonymization", "scrambling"]
---

I've just released today an extension called [PostgreSQL Anonymizer] that will
mask or replace personally identifiable information (PII) or commercially
sensitive data from a PostgreSQL database.

The project is open source and available here :

<https://gitlab.com/daamien/postgresql_anonymizer>

[PostgreSQL Anonymizer]: https://gitlab.com/daamien/postgresql_anonymizer

<!--MORE-->

![PostgreSQL Anonymizer](https://raw.githubusercontent.com/daamien/blog/gh-pages/assets/img/postgresql_anonymizer.banner.gif)

I strongly believe in  a **declarative approach** of anonymization : the location
of sensitive information inside database and the rules to hide this information
should be declared directly using the Data Definition Language (DDL).
In the age of GDPR, developpers should specify an anonymization strategy
inside the table definitions, just like they specify data types, foreign
keys and constraints.

This project is a **prototype** designed to show the power of implementing
data masking directly inside PostgreSQL. Currently it is based on the `COMMENT`
statement (probably the most unused PostgreSQL syntax) and an event trigger.
In the near future, I'd like to propose a new syntax for dynamic data masking
([MS SQL Server already has it])

[MS SQL Server already has it]: https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking?view=sql-server-2017

The extension can be used to put dynamic masks on certain users or permanently
modify sensitive data. Various masking techniques are available : randomization,
partial scrambling, custom rules, etc.

__Here's a basic example__ :

Imagine a `people` table

```sql
=# SELECT * FROM people;
  id  |      name      |   phone
------+----------------+------------
 T800 | Schwarzenegger | 0609110911
```

Step 1. Activate the masking engine

```sql
=# CREATE EXTENSION IF NOT EXISTS anon CASCADE;
=# SELECT anon.mask_init();
```

Step 2. Declare a masked user

```sql
=# CREATE ROLE skynet;
=# COMMENT ON ROLE skynet IS 'MASKED';
```

Step 3. Declare the masking rules

```sql
=# COMMENT ON COLUMN people.name IS 'MASKED WITH FUNCTION anon.random_last_name()';

=# COMMENT ON COLUMN people.phone IS 'MASKED WITH FUNCTION anon.partial(phone,2,$$******$$,2)';
```

Step 4. Connect with the masked user

```sql
=# \! psql test -U skynet -c 'SELECT * FROM people;'
  id  |   name   |   phone
------+----------+------------
 T800 | Nunziata | 06******11
```


Of course this project is a work in progress. I need your feedback and
ideas ! Let me know what you think of this tool, how it fits your needs and
what features are missing.

You can either [open an issue] or send me a message at <daamien@gmail.com>.

[open an issue]: https://gitlab.com/daamien/postgresql_anonymizer/issues
