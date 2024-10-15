---
date: 2024-10-15 11:17:36
layout: post
title: PostgreSQL Anonymizer 2.0 - Generating Fake Data
category: english
tags: ["PostgreSQL","data","masking", "anonymization", "faking"]
---


[PostgreSQL Anonymizer]: https://postgresql-anonymizer.readthedocs.io/

After several months of development, version 2.0 of [PostgreSQL Anonymizer] has entered the beta phase, 
and this is an opportunity for us to launch a series of articles to present its new capabilities in preview!

For this first technical overview, let's see how to generate fake data (also known as "synthetic data").


<!--MORE-->

![Photo Credit Markus Spiske]({{ site.baseurl }}/assets/img/2024_pexels-markusspiske-3806754.jpg)

<img src="{{ site.baseurl }}/assets/img/logo_pg_anonymizer_picto.png" alt="Logo PostgreSQL Anonymizer" style="float: right; padding: 10px; width: 200px;">

### Why is it important?

[PostgreSQL Anonymizer] 2.0 offers a wide range of functions to generate fake but realistic data. 
These functions are useful for writing masking rules and replacing sensitive data with data that "looks real."

But that's not the only benefit of these functions. In fact, they can also be used from the very first steps 
of a new project: when designing a data model, it is essential to "populate" the tables with data so as not 
to start development "empty-handed."

Consider a new application that needs to rely on a classic `customer` table:

``` sql
CREATE TABLE public.customer (
    id          INT PRIMARY KEY,
    firstname   TEXT,
    lastname    TEXT,
    email       TEXT
);
```

We want to insert 2000 people into this table, but how can we do it since the 
application doesn't exist yet?

### Why is it complicated?

Obviously, inserting `John Doe` 2000 times into the table is not really an option! 
The challenge is to produce **realistic** and **context-appropriate** data.

For example, if the application is intended for adult users, we want the birth dates to be between 
1950 and 2006.

This is where [PostgreSQL Anonymizer] comes into play with its wide range of [randomization] 
functions and [fake data generators].

[randomization]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#randomization
[fake data generators]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#advanced-faking

``` sql
INSERT INTO customer
SELECT
	i*100+pg_catalog.random(0,99), -- avoid collisions !
	anon.dummy_first_name(),
	anon.dummy_last_name(),
	anon.dummy_free_email()
FROM generate_series(1,2000) i;
```

**Note**: The `random(x,y)` function is one of the new features of PostgreSQL 17! 
For earlier versions, the `anon.random_int_between(x,y)` function is an 
equivalent alternative.

In total, PostgreSQL Anonymizer provides more than 70 fake data generation functions! 
They all have the `dummy_` prefix. The complete list is available in the _Advanced Faking_ 
section of the documentation:

<https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#advanced-faking>

Which gives us:

``` sql
SELECT * FROM customer LIMIT 2;
 id  | firstname | lastname |        email
-----+-----------+----------+----------------------
 143 | Eloisa    | Beer     | mavis_ab@hotmail.com
 200 | Braden    | Hagenes  | ariel_nam@yahoo.com
```

Let's now imagine that the application needs to store phone numbers and is 
intended for the French market. We can use the `anon.dummy_phone_number()` function:

``` sql
SELECT anon.dummy_phone_number();
 dummy_phone_number
-------------------
 648-881-1114
```

This works, but the format does not match that of French phone numbers.

To obtain fake values adapted to the local context, we can simply add the 
`_locale` suffix to the function and specify that we want a French number:

``` sql
ALTER TABLE customer ADD COLUMN phone TEXT;
UPDATE customer 
  SET phone = anon.dummy_phone_number_locale('fr_FR');
```

Et voilà ! :)

``` sql
SELECT * FROM customer LIMIT 2;
   id   | firstname | lastname |        email         |     phone
--------+-----------+----------+----------------------+----------------
 115076 | Zelda     | Robel    | afton_eos@gmail.com  | 05 38 16 52 85
 123886 | Kamille   | Ernser   | kiera_ut@hotmail.com | 04 24 18 60 76
```

In total, as of today, 7 locales are available partially or fully: 
ar_SA, en_US, fr_FR, ja_JP, pt_BR, zh_CN, zh_TW.

As you can see, generating high-quality fake data is no easy task! Rather 
than developing this feature *ex-nihilo* in [PostgreSQL Anonymizer], we chose to rely on 
the [fake-rs] library, which supports numerous locales and a wide range of categories.

[fake-rs]: https://github.com/cksac/fake-rs

### The PostgreSQL Faker project is now end-of-life

If you have already used [PostgreSQL Anonymizer], you might also be aware of the [PostgreSQL Faker] 
extension, which previously addressed the need for fake data. For various reasons, we have decided 
to freeze this project and integrate this functionality directly into [PostgreSQL Anonymizer].

The [PostgreSQL Faker] project is now deprecated, and all its users are encouraged to gradually 
migrate to [PostgreSQL Anonymizer] 2.0.

[PostgreSQL Faker]: https://gitlab.com/dalibo/postgresql_faker

### Your turn!

All these new [fake data generators] are still in the beta phase! Feel free to test them 
and share your feedback and use cases. How do you generate fake data to populate your tables? 
Does this new feature meet your needs? Etc.

For any requests or ideas, open a ticket via the link below and let's discuss!

<https://gitlab.com/dalibo/postgresql_anonymizer/-/issues>

> PS: This article was translated with the assistance of artificial intelligence.
> The original version in French is available here*

<https://blog.dalibo.com/2024/10/10/postgresql_anonymizer_generer_des_donnees_factices.html>