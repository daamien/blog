---
date: 2015-03-15 15:17:36 
layout: post
title: "70 Shades of Postgres"
description: "PostgreSQL as a Data Integration Platform"
category: english
tags: [PostgreSQL,FDW,ETL]
---

Support for the SQL/MED standard was introduced in PostgreSQL 9.1 (2011). Four years later, we now have more the 70 Foreign Data Wrappers (FDW) available, 
which you can use PostgreSQL to read and write on type of data storage : Oracle, MongoDB, XML files, Hadoop, Git repos, Twitter, you name it, ...

<!-- More -->

![](https://raw.githubusercontent.com/daamien/blog/gh-pages/_images/catwrap.jpg)

A few days ago, during FOSDEM I attended a talk by my colleague Ronan Dunklau about [Foreign Data Wrappers in PostgreSQL : Where are we now ?](https://fosdem.org/2015/schedule/event/foreign_data_wrappers_in_postgresql_where_are_we_now/). Ronan is the author of the [multicorn](http://multicorn.org/) extension and during his talk I couldn't 
help but thinking the multicorn is probably one of the most underrated piece of code in the PostgreSQL Community. As a quick exercise I started to count all PostgreSQL FDW 
I knew about... and soon realized there were too many to fit my small memory.

So I went back to the [FDW page in PostgreSQL wiki](https://wiki.postgresql.org/wiki/FDW?nocache=1) and started to update and clean up the catalog of all the existing FDW 
and ended with a list of 70 wrappers for PostgreSQL... 


## Lessons learned 

During this process I learned a few things :

* Almost all the [major RDBMS](https://wiki.postgresql.org/wiki/FDW#Specific_SQL_Database_Wrappers) are covered, except DB2. Strangely DB2 is also the only other RDBMS with a SQL/MED implementation. 
I'm not sure if those 2 facts are related or not.

* One third of the FDW are written in Python and based on Multicorn. The others are "native" C wrappers. Obviously C wrappers are prefered for 
the [database wrappers](https://wiki.postgresql.org/wiki/FDW#Specific_SQL_Database_Wrappers) (ODBC, Oracle, etc.) for performance reasons. Meanwhile Multicorn is mostly used for [query web services](https://wiki.postgresql.org/wiki/FDW#Generic_Web_Wrappers) (S3 storage,RSS files, etc.) and [specific data formats](https://wiki.postgresql.org/wiki/FDW#Scientific_Wrappers) (Genotype files, GeoJSON, etc.) 

* I'm not sure they were meant to be this way, but Foreign Data Wrapper are also used to innovate. Some wrappers are diverted from the original purpose to implement new features. For instance, CitusDB has released a [column-oriented storage](https://wiki.postgresql.org/wiki/FDW#Column-Oriented_Wrappers), others wrappers are going in more [exotic directions](https://wiki.postgresql.org/wiki/FDW#Exotic_Wrappers) such as an OS level stat collector, GPU acceleration for seq scan, or a distbributed paralelle query engine, ... Most of these project are not suited for production, of course. However it shows how easy you can implement a Proof of Concept and how versatile this tehcnology is...

* The PGXN distribution network doesn't seem to be well-kown among FDW developpers. Only 20% of the wrappers are available via PGXN. 
Or developers are too lazy to package their extensions ?

* The success of PostgreSQL SQL/MED implementation comes with a drawback. My guess is that there will be more 100 data wrappers available by the end of 2015. For some data source like mongodb or LDAP, there's already several different wrappers, not to mention the procession of Hadoop connectors :)In the long term, PostgreSQL end users might get confused by all these wrappers and it will be hard to know which one is maintained and which one is obsolete... The wiki page tries to answer that problem but there may be other solutions to provide information to the end user... Maybe we need a Foreign Data Wrapper that would list all the Foreign Data Wrappers :) 


## The next big thing : IMPORT FOREIGN SCHEMA 

Moreover there's still a big limitation th the current SQL/MED implementation : **metadata**. Right now, there's no simple way to use the schema introspection capabilities of the foreign data source. This means you have to create all the foreign tables one by one. When you want to map all the tables of a distant database, it can be time-consuming and error-prone. If you connectinig to a remote PostgreSQL instance you can use tricks like [this one](https://news.ycombinator.com/item?id=8027472) but objectively this is a job the Wrapper should do by itself (when possible).

Here's comme the new ``IMPORT FOREIGN SCHEMA`` statement ! This new features was written by Ronan Dunklau, Michael Paquier and Tom Lane. You can read a quick demo at [Michael's blog](http://michael.otacoo.com/postgresql-2/postgres-9-5-feature-highlight-import-foreign-schema/). This will be available in the forthcoming PostgreSQL 9.5.

This is a huge improvement ! This ``IMPORT`` feature combined and dozens of Wrappers available are 2 key factors : PostgreSQL is becoming a solid Data Integration Plateform and it reduces the need for external ETL software. 

![PostgreSQL as a Data Integration Plateform](https://raw.githubusercontent.com/daamien/blog/gh-pages/_images/800px-Dataintegration.png)

**Links :**

* [FDW wiki page](https://wiki.postgresql.org/wiki/FDW?nocache=1)
* [multicorn](http://multicorn.org/)
* [IMPORT FOREIGN SCHEMA](http://www.postgresql.org/docs/devel/static/sql-importforeignschema.html)

