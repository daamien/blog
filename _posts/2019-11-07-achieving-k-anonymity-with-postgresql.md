---
date: 2019-11-07 11:17:36
layout: post
title: "Achieving k-anonymity with PostgreSQL"
description: "Data Generalization using RANGE datatypes"
category: english
tags: ["PostgreSQL","data","masking", "anonymization", "scrambling"]
---

[Dalibo Labs] just released yesterday the [version 0.5] of 
[PostgreSQL Anonymizer], an extension that hides sensitive 
information inside a PostgreSQL database.

[Dalibo Labs]: https://labs.dalibo.com
[version 0.5]:  FIXME
[PostgreSQL Anonymizer]: https://gitlab.com/dalibo/postgresql_anonymizer/

<!--MORE-->

The extension already has a large panel of anonymization strategies: [randomization],
[faking], [partial destruction], [shuffling], [noise addition], etc.

For this new released, I worked on a different approach called [Generalization] and 
the [k-anonymity] property.

[randomization]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#randomization
[faking]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#faking
[partial destruction]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#partial-scrambling
[shuffling]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#shuffling
[noise addition]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#adding-noise
[Generalization]: https://postgresql-anonymizer.readthedocs.io/en/latest/generalization/
[k-anonymity]: https://postgresql-anonymizer.readthedocs.io/en/latest/generalization/#k-anonymity

## Generalization

The idea behing Generalization is quite simple: To avoid giving a sensitive 
information, we can replace the data with a broader, less accurate value. 

For instance, instead of saying "Bob is 28 years old", you can say 
"Bob is between 20 and 30 years old". This is something we all do unconsciously
when we don't want to reveal a precise information. Typically when someone ask 
"Where do you live ?", we often respond with a vague answer ("I live in Paris" 
or even "I live in France") instead of giving our exact address.

What is interesting with this approach is that **the data remains true** while 
avoiding the risk of re-identification.

So [Generalization] is an instinctive and simple way to hide personnal 
information for
human beings. But how would that apply to a database management system like 
PostgreSQL ? After all the primary purpose of a database is to store accurate 
information. If you have `birthdate` column in a SQL table, you can't 
practically insert data saying "sometime in the nineties", right ? 😀

Well... it turns out that PostgreSQL can handle generalization very easily with 
the [RANGE] data types, a very powefull way to store and manipulate a set of 
values contained between a lower and an upper bound. 

[RANGE]: https://www.postgresql.org/docs/current/rangetypes.html


Example: Blurring medical data
--------------------------------------------------------------------------------

Here's a basic table containing sensible health data:

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

We want the anonymized data to remain **true** because it will be
used for statistics. We can build a generalized view upon this table 
with 2 mains actions : 

1. First, we need to get rid of primary keys and direct identifiers.
   Here we will simply remove the `ssn` field and destroy the content 
   of the `firstname` field.  

2. Second, we will reduce the accuracy of the [indirect identifiers] 
   ( `zipcode` and `birth`) using 2 generalization functions from the 
   PostgreSQL Anonymizer extension : `anon.generalize_int4range` and 
   `anon.generalize_daterange`.

Here we go: 

```sql
CREATE MATERIALIZED VIEW generalized_patient AS
SELECT
    'REDACTED'::TEXT AS firstname,
    anon.generalize_int4range(zipcode,1000) AS zipcode,
    anon.generalize_daterange(birth,'decade') AS birth,
    disease
FROM patient;
```

Notice that the [generalization functions] take 2 arguments: the 
original value as the first parameter and a second parameter for 
the length of each step of the range.

[generalization functions]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/#generalization

For numeric values, the step is an integer. Here the `zipcode` column
is generalized in steps of 1000. For datetime values, the possible steps 
are: microseconds, milliseconds, second, minute, hour, day, week, month,
year, decade, century and millennium. Here the `birth` column is generalized
in decades.

This will give us this less accurate view of the data:

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

The nice thing with [RANGE] datatypes is that they are very easy to use 
for statistics. For instance we can answer like this to the question 
"How many people born before 1990 have had flu ?": 

```sql
SELECT count(*)                                                                  
FROM generalized_patient                                                         
WHERE disease = 'Flu'                                                        
AND upper(birth) < '1990-01-01';
```

As you can see, if you generalize the data with wider steps, the data will 
be less accurate. For instance, using `millennium` in the 
`anon.generalize_daterange()` function would completely destroy the data. 
On the hand, using `week` would be too precise and there would be risk of 
re-identification. 

So the main question that comes with [Generalization] is : "Is this dataset
generalized enough ?" and this is where we meet the concept of [k-anonymity].

## k-anonymity


[k-anonymity] is an industry-standard term used to describe a property of an 
anonymized dataset. The k-anonymity principle states that within a 
given dataset, any anonymized individual cannot be distinguished from at 
least `k-1` other individuals. In other words, k-anonymity might be described 
as a "hiding in the crowd" guarantee. A low value of `k` indicates there's 
a risk of re-identification using linkage with other data sources.

With [PostgreSQL Anonymizer], you can evaluate the `k` factor of a generalized 
table in 2 steps :

1. First declare the columns that are [indirect idenfiers] ( also known
   as "quasi identifers") like this:

```sql
SECURITY LABEL FOR anon ON COLUMN patient.zipcode IS 'INDIRECT IDENTIFIER';
SECURITY LABEL FOR anon ON COLUMN patient.birth IS 'INDIRECT IDENTIFIER';
```

2. Once the indirect identifiers are declared, just use the `k_anonymity()`
   function:

```sql
SELECT anon.k_anonymity('generalized_patient')
```

In the example above, the `k` factor is 3, which means that for any 
value of (`zipcode`,`birth`), you will find at least 3 individuals in the 
`generalized_patient` table. If `k = 1`, some people in your data set may 
be reidentified using external data or inference techniques.


## Different anonymization strategies for different purposes

You may have already spotted 2 main limitations of Generalization strategy : 


A/ This technique works great low-dimensional datasets (i.e. tables with a small 
   number of columns). But if a datasets contains a lot of [indirect identifiers]
   the `k` factor will drop

B/ The generalized view and the original table don't have the same data model. 
   In particular, the numeric and datetime value will be replaced by [RANGE]. 
   Since [Generalization] breaks the data model, it is not possible to use 
   [Dynamic Masking], [Anonymous Dumps] and [In-Place Anonymization] with this
   strategy. In other words: [Generalization] cannot be used for CI tests, for
   application development or as training data. 

[Masking Functions]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/
[Anonymous Dumps]: https://postgresql-anonymizer.readthedocs.io/en/latest/anonymous_dumps/
[In-Place Anonymization]: https://postgresql-anonymizer.readthedocs.io/en/latest/in_place_anonymization/
[Dynamic Masking]: https://postgresql-anonymizer.readthedocs.io/en/latest/dynamic_masking/
[indirect identifiers]: https://labkey.med.ualberta.ca/labkey/_webdav/REDCap%20Support/@wiki/identifiers/identifiers.html?listing=html

This is the demonstration that there is no "1-size-fits-all" approach for 
anonymization ! In fact, in some case you may have to build several 
anonymization workflows for a given dataset, because different data operators 
will have different needs and constraints.

This is all for today ! If GDPR and Anonymization are a concern to you, please 
take a look at the [PostgreSQL Anonymizer] extension and **send us feedback** ! 

We love feedback 😀

The project is open source and available here:

https://gitlab.com/dalibo/postgresql_anonymizer/
