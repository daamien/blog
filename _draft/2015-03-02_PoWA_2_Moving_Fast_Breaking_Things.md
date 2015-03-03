---
date : 2015-03-02 20:03:14
layout: post
title: PoWA 2 ! Moving Fast and Breaking Things
category: english
tags: [PostgreSQL, performance, workload]
---

Yesterday, DALIBO release the second major version of [PoWA](http://dalibo.github.io/powa/), the PostgreSQL Workload Analyzer. 
We're kind of proud of what we've achieved with this new version, however we did crack some eggs while making that omelette...
H
ere's a brief explanation for all the disruptives choices we made...

<!-- MORE -->

When we launched the [PoWA](http://dalibo.github.io/powa/) project in 2014, we decided to release the project very early and see where it would lead us to. Over the last few months, we took some time to analyze the situation. We looked at the users feedback. We tried to imagine how the project would interact with the other monitoring and performance tools we're working on, especially [OPM](http://opm.io), [pg_stat_kcache](https://github.com/dalibo/pg_stat_kcache) and [pg_qual_stats](https://github.com/dalibo/pg_qualstats) 

![http://xkcd.com/1428/](http://imgs.xkcd.com/comics/move_fast_and_break_things.png)

We came to the conclusion that in order to move forward, we had to make a lot of disruptive technical choices. Please fasten your seatbelt:

* We're splitting the code in 2 sub-projects : The PostgreSQL extension is now called [PoWA-archivist](https://github.com/dalibo/powa-archivist) and the graphic interface is now called [PoWA-web](https://github.com/dalibo/powa-web). 

* We're switching to [Semantic Version](http://semver.org/) to avoid dependency nightmares. In particular, you will have the guarantee that minor versions of the sub-projects are compatible. For instance, [PoWA-archivist](https://github.com/dalibo/powa-archivist) 2.0.1 will be compatible with [PoWA-web](https://github.com/dalibo/powa-web) 2.0.9

* We're dropping the [mojolicous](http://mojolicio.us/) web framework in favor of [tornado](http://www.tornadoweb.org/). There's a lot we could say about this, but in a nutshell we found that mojolicious release cycle is too erratic for us and switching to Python makes it easier to open the 
project to other contributors. And yes this means we rewrote entirely the GUI code from scratch.

* We're dropping support for PostgreSQL 9.3 and from now on the new versions of [PoWA](http://dalibo.github.io/powa/)  will only work with the 9.4 versions and later. This was a hard choice but if we want to implement ground-breaking feature like the [Missing Index Finder](https://github.com/dalibo/powa/wiki/2015-03-02-PoWA_2_Finding_the_missing_index), we need to use the latest capacities of PostgreSQL. That being said, we will continue to maintain [PoWA](http://dalibo.github.io/powa/)  v1.2 and you can continue to use it on your PostgreSQL 9.3 servers.

* We're allowing multiple source of stats. So far [PoWA](http://dalibo.github.io/powa/) is able to collect stats from 3 different extensions : [pg_stat_statements](http://www.postgresql.org/docs/current/static/pgstatstatements.html) ,  [pg_qualstats](https://github.com/dalibo/pg_qualstats) and [pg_stat_kcache](https://github.com/dalibo/pg_stat_kcache). If you've developed a stat extension for PostgreSQL, integrating it into PoWA should be a [piece of cake](http://powa.readthedocs.org/en/latest/powa-archivist/development.html#integration-with-powa).
 
We know that all these changes may be confusing at first, especially for people using PoWA with PostgreSQL 9.3. However we believe that the disruptive changes we're making now will enable us to build great tools in the forthcoming months. 

If you have any question about the [PoWA](http://dalibo.github.io/powa/)  project, how we work and where we're going, please join us on the [powa-users mailing list](https://groups.google.com/forum/?hl=fr#!forum/powa-users) !


