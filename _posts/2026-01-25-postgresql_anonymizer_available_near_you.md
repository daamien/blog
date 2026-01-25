---
date: 2026-01-25 12:30:36
layout: post
title: "PostgreSQL Anonymizer, available in all good shops"
description: "The cooperative company DALIBO is celebrating its 20th anniversary today !"
category: english
tags: ["PostgreSQL"]
---

As we prepare for the upcoming release of PostgreSQL Anonymizer 3.0, I took some time to check 
which platforms now support the extension. What I discovered brought me a sense 
of achievement that I wanted to share with the community.

![](https://raw.githubusercontent.com/dalibo/blog/gh-pages/img/PostgreSQL-Anonymizer_H_couleur.png)

## More and More Platforms Are Embracing Data Anonymization

Over the past months, several major Cloud Service Providers have adopted 
the PostgreSQL Anonymizer extension, making it easier than ever for 
organizations to protect sensitive data.

The new adopters include:

* [Aiven]
* [IBM Cloud]
* [Yandex]

They add to the current list composed of [Alibaba Cloud], [Crunchy Bridge], 
[Google Cloud SQL], [Microsoft Azure Database], [Neon] and [others]

[Aiven]: https://aiven.io/blog/using-postgresql-anonymizer-to-safely-share-data-with-llms
[Alibaba Cloud]: https://www.alibabacloud.com/help/en/rds/apsaradb-rds-for-postgresql/extensions-supported-by-apsaradb-rds-for-postgresql
[Crunchy Bridge]: https://access.crunchydata.com/documentation/postgresql-anonymizer/latest/
[Google Cloud SQL]: https://cloud.google.com/sql/docs/postgres/extensions#postgresql_anonymizer
[IBM Cloud]: https://cloud.ibm.com/docs/databases-for-postgresql?topic=databases-for-postgresql-data-masking
[Microsoft Azure Database]: https://learn.microsoft.com/fr-fr/azure/postgresql/flexible-server/concepts-extensions
[Neon]: https://neon.tech/docs/extensions/postgresql-anonymizer
[Postgres.ai]: https://postgres.ai/docs/database-lab/masking
[Yandex]: https://yandex.cloud/en/docs/managed-postgresql/operations/extensions/pg_anon
[others]: https://postgresql-anonymizer.readthedocs.io/en/latest/INSTALL/#install-in-the-cloud


## Growing Support Across PostgreSQL Forks

Perhaps even more remarkable is the adoption by major PostgreSQL forks 
and enterprise distributions. Each of these platforms has its own specific requirements 
and user base, and seeing PostgreSQL Anonymizer integrated across this ecosystem 
is truly humbling:

* [EDB Postgres]
* [Postgres Pro Enterprise]
* [Tanzu Greenplum]
* [Yugabytes]

Please refer to their own documentation on how to activate the extension as they
might have a platform-specific install procedure.

[Postgres Pro Enterprise]: https://postgrespro.com/docs/enterprise/current/pgpro-anonymizer
[Tanzu Greenplum]: https://techdocs.broadcom.com/us/en/vmware-tanzu/data-solutions/tanzu-greenplum/7/greenplum-database/ref_guide-modules-postgresql_anonymizer.html
[EDB Postgres]: https://www.enterprisedb.com/docs/pg_extensions/pg_anonymizer/
[Yugabytes]: https://docs.yugabyte.com/stable/additional-features/pg-extensions/extension-pganon/


## Beyond PostgreSQL: The Django Integration

I've also noticed a [Django plugin for PostgreSQL Anonymizer](https://django-postgres-anonymizer.readthedocs.io/en/latest/), 
making it easier for Python developers to integrate data anonymization 
into their applications. 


## Reflecting on  our journey

When we started working on PostgreSQL Anonymizer in 2018, the goal was simple: 
provide a straightforward way to mask personal information directly within PostgreSQL. 
We wanted to make privacy-preserving techniques accessible to anyone using PostgreSQL, 
without requiring complex and expansive external tools or ETLs.

Seeing this level of adoption across cloud providers, enterprise distributions, and 
even extending into application frameworks is incredibly rewarding. But it's important to 
remember that this success belongs to everyone who contributed to the project.

I want to express my deepest gratitude to all the contributors who have submitted patches, 
reported bugs, improved documentation, and provided feedback over the years. 
And espacially to my colleagues who have supported and encouraged this work. 

## Looking Ahead

As we move toward the 3.0 release, this growing adoption motivates us to keep improving 
and maintaining the extension to the highest standards. The diversity of platforms now 
supportung the extension shows that protecting users privacy is now a global concern.

The journey continues, we actively working on supporting even more platforms such as SUSE,
the CNPG operator and others. If you want to use the extension for your project, but 
you can't install it for whatever reason, please don't hesitate to reach out at 
contact@dalibo.com !

