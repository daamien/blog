---
date: 2019-09-23 10:17:36
layout: post
title: New version of PostgreSQL Anonymizer and more...
description: "GDPR fines are coming and other stories !"
category: english
tags: ["PostgreSQL","data","masking", "anonymization", "GDPR"]
---

One year I started a side-project cvalled [PostgreSQL Anonymizer] to study 
and learn various ways to protect privacy using the power of PostgreSQL. 
The project is now part of the [Dalibo Labs] intiative and we've published
a [new version] last week...

<!--MORE-->

This seems like a nice moment to analyze the progress we've made, how the GDPR 
is changing the game and where we're going....

[PostgreSQL Anonymizer]: https://gitlab.com/dalibo/postgresql_anonymizer
[Dalibo Labs]: https://labs.dalibo.com
[new version]: http://blog.dalibo.com/2019/09/13/postgresql_anonymizer_0.3_EN.html


![](https://raw.githubusercontent.com/dalibo/blog/gh-pages/img/PostgreSQL-Anonymizer_H_couleur.png)

## GPDR : Sanctions are coming 

While I was working on this, the landscape has changed... When the GPDR was 
implemented in May 2018, one the biggest questions was if the fines would be 
signification enough to force a real change in corporate data policies....

From what we can see, the fines are [GPDR fines are starting to fall], just 
during July 2019 : Bristish Airways got 204 M€ and Marriott Hotels got 110 M€.

There are also smaller fines for smaller companies, what's interesting is that 
the biggest fines are related to the [Article 32] and the « Insufficient 
technical and organisational measures to ensure information security »

In other words : **Data Leaks**.

Here's where anonymisation can help ! Based on my experience, we can reduce 
the risks of leaking personnal information by limiting the number of 
environments where the data is hosted. In many staging setups such as 
pre-production, training, development, CI, analytics, etc... the real data
is not absolutely required. With a strong anonymization policy we can limit 
real data only where it is needed and work on fake/random data everywhere 
else. When Anonymization is done the right way, the anonymized datasets are 
not concerned by the GPDR. 

In a nutshell, anonymization is powerful method to reduce your 
**attack surface** and its a key to limit the risks GPDR penalties related to 
data leaks.

This is why we're investing a lot of effort to develop masking tools directly 
inside PostgreSQL !

[Art. 32]: http://www.privacy-regulation.eu/en/32.htm
[GPDR fines are starting to fall]: http://www.enforcementtracker.com/

## Major Improvements 

Over the last month, I've worked on different aspect of the extension, especially :

* Adding more tests and protections against SQL injection
* Enabling users to export [Anonymous Dumps] using the wonderful 
  [ddlx] extension
* Adding more [Masking Functions], in particular `shuffling` and `noise
  insertion`
* Rebuilding large parts of the [Dynamic Masking] engine
* Clarifying the concept of [In-Place Anonymization]
* Writing a better [documentation] :-)



[ddlx]: https://github.com/lacanoid/pgddl
[documentation]: https://postgresql-anonymizer.readthedocs.io/
[Masking Rules]: https://postgresql-anonymizer.readthedocs.io/en/latest/declare_masking_rules/
[Masking Functions]: https://postgresql-anonymizer.readthedocs.io/en/latest/masking_functions/
[Anonymous Dumps]: https://postgresql-anonymizer.readthedocs.io/en/latest/anonymous_dumps/
[In-Place Anonymization]: https://postgresql-anonymizer.readthedocs.io/en/latest/in_place_anonymization/
[Dynamic Masking]: https://postgresql-anonymizer.readthedocs.io/en/latest/dynamic_masking/




## Security Labels

One of the main drawback of the current implementation of PostgreSQL is that 
[Masking Rules] are declared using the `COMMENT` syntax which can be annoying
if your database already has comments. 

Thanks to an idea from Alvaro Herrera, I'm currently working on a new declaration syntax based on [Security Labels], a little known feature 
also used by the [sepgsql] extension

[sepgsql]: https://www.postgresql.org/docs/current/sepgsql.html

[Security Labels]: https://www.postgresql.org/docs/current/sql-security-label.html

```sql
SECURITY LABEL FOR anon ON COLUMN people.zipcode
IS 'MASKED WITH FUNCTION anon.fake_zipcode()'
```

This should be available in a few weeks. Of cource, the former syntax will  
still be supported for backward compatibility.

## Let's talk !

GDPR and data privacy are two very hot topics ! I will be talking about those 
subjects in various events in the forthcoming weeks :

* on October 14th at [PostgreSQL Conference Europe] in Milan, Italy
* on November 14th at [Libday / Devops D-Day] in Marseille, France 

[PostgreSQL Conference Europe]: https://www.postgresql.eu/events/pgconfeu2019/schedule/session/2780-anonymization-gdpr-and-beyond/
[Libday / Devops D-Day]: https://2019.libday.fr/programme/

If you have any ideas or comments on PostgreSQL Anonymizer or more generally 
about protecting data privacy with progress, please send me a message at 
<damien@dalibo.com>.
