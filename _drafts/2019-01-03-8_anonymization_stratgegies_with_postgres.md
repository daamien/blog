---
date: 2017-10-27 08:17:36
layout: post
title: "8 Anonymization Strategies with PostgreSQL"
description: ""
category: french
tags: [PostgreSQL, data, anonymization, privacy]
---

Data Anonymization is a complex topic but PostgreSQL has a lot of interesting
features to tackle this challenge! Here is an overview of different approach and
how to implement them directly within a PostgreSQL database.

<!--MORE-->

Over the last few month I have been working on a project called
[PostgreSQL Anonymizer]. It led me to try various techniques to remove
personal data for different purposes: development, CI, functional testing,
Analytics, etc.

[PostgreSQL Anonymizer]: https://gitlab.com/dalibo/postgresql_anonymizer
[Introducing PostgreSQL Anonymizer]: http://blog.taadeem.net/english/2018/10/29/Introducing-PostgreSQL-Anonymizer

All the examples in the article will use a simplified table (see below) and
should work with all current versions of PostgreSQL (from 9.4 to 11).

```SQL
CREATE TABLE people (
    id SERIAL,
    name TEXT NOT NULL,
    address TEXT,
    age INTEGER,
    salary INTEGER,
    phone TEXT
);
```


## 0. Sampling

Sampling is not Anonymization! But when you're looking to remove personal data
from a database, most of the time you don't need to publish all the rows.
So before going any further it is important to note that PostgreSQL provides
a feature called `TABLESAMPLE` that will reduce the size of your dataset.

For example, if you want to work only on 20% of a table:

```SQL
SELECT * FROM people TABLESAMPLE BERNOULLI(20);
```

And if you want to extract a subset among several tables while maintaining
referential integrity, [pg_sample](https://github.com/mla/pg_sample) is your
friend.


## 1. Suppression

This is the most obvious way to get rid of personal identifiers: just wipe
a column by replacing all values with `NULL` (aka "Nullification") or with a
constant value ("Static Substitution").

Example :

```SQL
UPDATE people SET name = '<CONFIDENTIAL>';
UPDATE people SET address = NULL;
```

This is simple and fast. For certain data fields, it may be the best option.

But of course it will break integrity constraints (`PRIMARY, ``UNIQUE`,
`NOT NULL`, etc.).  And moreover, the column will be useless for functional
testing.



---

## 2. Random Substitution

This time we are replacing personal data with purely random values.

Example:

```SQL
UPDATE people SET name = md5(random()::text);
UPDATE people SET salary= 100000*random();
```

Again this is simple and it does not break `NOT NULL` constraints.
But this is still useless for functional testing and for analytics...


---

## 3. Variance

The idea here is to add noise to the data by "shifting" the values from the
original dataset and make them to be slightly different.

For example, modify the salary field by a random factor of +/- 25% like this:

```SQL
UPDATE people
SET salary= salary *  (1+ (2 * random() - 1 ) * 0.25) ;
```

This is great for indirect identifiers: dates and numeric values that should
remain meaningful without revealing identity. Aggregations such as
`average` and `count` will be similar to the original

Of course, it is important to adapt the degree of perturbation depending on
the distribution of the values, of the attribute and the size of the dataset.

However even with a strong perturbation, reidentification is possible when the
dataset contain extreme data. In the previous example, if an employee earns 100
times the average wage, adding noise will not hide that such a big difference
and it is easy to deduce that the person is the CEO of the company.


---


## 4. Encryption

Anonymization and Encryption are often associated when people talk about data
privacy. But each process has its own purpose. The main difference being that
Encryption is a two-way process (some user should be able decrypt or re-hash
the data), while Anonymization is a definitive alteration of the dataset.

It is possible to use encryption function as a destructive method just
by immediately destroying the encryption key. PostgreSQL offers a wide range of
encryption functions packed in an extension called [pgcrypto].


[pgcrypto]: https://www.postgresql.org/docs/current/pgcrypto.html

For example, a simple and definitive way is to alter data is to
generate a new random "salt" for each call a the `crypt` function:

```SQL
CREATE EXTENSION pgcrypto;

UPDATE people
SET name = SELECT crypt('name', gen_salt('md5'));
```

In my view, this is useful for very specific situations: mostly
TEXT attributes with an UNIQUE constraint, or when you need the transformation
process to be `IMMUTABLE` (but then if the salt or encryption key is stolen,
authentic data can be revealed).

Anyway, this strategy is a good fit for analytics. Not for functional testing
or development, because in the long run, it is hard to work with values like
"DN29BHSY$CVju1" and "S7ckeXJAVYZfM3SF1" :-)


---


## 5. Shuffling

The purpose of shuffling is to swap data within the dataset. Each value of the
column is still present, but attached to a different record.

For example, here's a permutation on the `salary` column:

```SQL
WITH p1 AS (
    SELECT row_number() over (order by random()) n,
           salary AS salary1
    FROM people
),
p2 AS (
    SELECT row_number() over (order by random()) n,
           id AS id2
    FROM people
)
UPDATE people
SET salary = p1.salary1
FROM p1 join p2 on p1.n = p2.n
WHERE id = p2.id2;
```

This is less destructive approach, because we keep real values in the dataset:
aggregates (`sum`, `average`) are correct. This is also the best strategy if
the column is referenced by a foreign key.

However the shuffling function is a bit complex to write and it is not really
effective on data with low standard deviation or very few distinct values
(such as `boolean`).

---


# 6. Faking / Mocking

Another idea is to replace sensitive data with **random-but-plausible**
values, also know as `synthetic data`.

For instance, we can call a function `fake_address()` that will produce
a value that looks real :

```sql
UPDATE people
SET address = fake_address();
```

The difficulty, of course, is to write the faking function. For dates or
numeric values, it is basic. But for more complex data types, it may require
some effort to produce relevant synthetic data.

Furthermore this technique is not appropriate for analytics because the values
are not "real". On the other hand, it is perfect for CI and functional testing.

[faker]: https://github.com/joke2k/faker

---


# 7. Partial Suppression

In certain cases, you might want to hide just some parts of the data and leave
other parts visible.

For instance, let's replace the following  phone number "01 42 92 81 00"
by "XX XX XX 81 00":

```sql
UPDATE people
SET phone = 'XX XX XX ' || substring(phone FROM 9 FOR 5 );
```

Partial Suppression is similar to Static Substitution. But it has an
interesting property: the data subjects can still recognize his/her own data
while other users can't...

Another advantage is that this transformation is `IMMUTABLE`: the same
input values will always produce the output.

This technique is perfect for direct identifiers: Credit Card, Phone, E-mail,
etc.

However it works only with `TEXT` / `VARCHAR` types.

---

## 8. Generalization

With this approach, values are replaced with a broader category. This is
easily done in Postgres with `RANGE` data type.

For instance, we can replace the `age` field with a range based on the previous
and next decade. Instead of saying "Person X is 28 years old", you'd say
"Person X is between 20 and 30 years old."

```SQL
CREATE TABLE anonymous_people
AS  SELECT
        id,
        '*' AS name,
        int4range(age/10*10, (age/10+1)*10) AS age
    FROM people
;
```

The main drawback here is the change of data type. The `age` is now a range of
numeric values. Obviously this will break CI, functional tests and any use
related to the application. But this approach is fine for data analytics and
aggregation.

We also face the same issue as the `variance` strategy: if the data set has
'extreme values' and the generalization is too soft, there's a risk of
re-identification. With the example above, if there's only 1 person under 20
years old in the table, he/she will be easily recognized.

---

# Finding the right strategy

In a nutshell, anonymization is a complex topic and it takes time to produce an
anonymized dataset that would at the same time useful and with a low risk of
reidentification.

For the same dataset, you might need to use different strategies depending on
the final destination of the data.

Here's a quick recap :

| Strategy            | Data Types       | When to use                        |
|---------------------|------------------|------------------------------------|
| Suppression          | All              | Useless attributes                 |
| Random Substitution | All   | Useless attributes with integrity constraints |
| Variance            | Numeric / Dates  |  Analytics |
| Encryption          | Text             | Analytics / UNIQUE attributes |
| Shuffling           | All              | Analytics              |
| Faking / Mocking    | All              | Dev / CI / Functional Testing  |
| Partial Suppression | Text             | Direct Identifiers |
| Generalization      | Numeric / Dates  | Analytics |

What we are trying to do with [PostgreSQL Anonymizer] project is to provide
tools to help developers implement those techniques. We will work mainly on
shuffling, variance, faking and dynamic masking. The goal is to extend the DDL
syntax and define the masking policy directly inside the table declaration. The
project is at an early stage of development. We are really looking for feedback
and ideas on how to proceed.

If you are interested, check out the code here :
<https://gitlab.com/dalibo/postgresql_anonymizer>
