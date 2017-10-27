---
date: 2017-10-30 08:17:36
layout: post
title: Let's build better graphic tools for PostgreSQL 
description: "A quick report from the PostgreSQL Graphical Tools Developers Meeting"
author: damien clochard
twitter_id: @daamien
github_id: daamien
category: english
tags: [PostgreSQL, GUI ]

---

The very first [PostgreSQL Graphic Tools Developer
Meeting](https://wiki.postgresql.org/wiki/PostgreSQL_Graphic_Tools_Developper_Meeting_2017) 
happened last week during [PostgreSQL Conference Europe 2017](https://2017.pgconf.eu) 
in Warsaw. The meeting gathered 16 developpers from 14 software (KEXI, Postico,
Postgres.app, pgAdmin, DBeaver, SQL Tabs, pg_view, bg_mon, pgBadger, pgFormatter,
Postgres Professional Manager, temBoard, PoWA, pgDevOps, temBoard) and 11 different
organizations (EnterpriseDB, OpenSCG, PostgresPro, EggerApps, Crunchy Data, Dalibo,
EnterpriseDB, KDE, 2nd Quadrant, Rider Soft, Zalando)

Here's a quick summary.

<!--MORE-->

PostgreSQL is the best open source database ever and its community is packed
with brilliant people. But there's always room for improvements and the graphic
tools is the main area where we need to focus for the forthcoming years. By
"PostgreSQL Graphic Tools" I mean any software that provides a visual access to
Postgres in large sense. It can be an administration tool, a monitoring tool, an installer or a reporting tool.    

The meeting we had in Warsaw was a first step in this direction and it revealed
5 big issues we are currently facing

We are divided
-------------------------------------------------------------------------------

The meeting is just a small part of the whole picture. When we prepared this
event with [leo cossic](https://github.com/leorenc) we identified more than 60 developers 
actively working on 35 different tools. This does not include the inactive
projects such as PhpPgAmin and the ones we forgot (apalogies to Datagrip and Elephant
Shed !).

Now let's compare this to other competitors : Oracle DBAs have `Grid Control` or
`TOAD`. MySQL users have `PhpMyAdmin` or `Workbench`. For MSSQL
Server, the `SQL Server Management Studio` is pretty much your only choice.

Of course open source is all about diversity, but at some point too much choice is
becoming a problem. New PostgreSQL users are getting really confused by this
profusion of tools and the comminty workforce is split in separate projects. All 
in all it is slowing down the PostgreSQL adoption.

__What can we do about it ?__ We need to raise awareness of the community and
make a better at promoting the existing solutions. The message must be clear :
if you want to build a PostgreSQL administration tool, please fork pgAdmin or
another one... But don't start a new one from scratch....


We don't talk to each other
-------------------------------------------------------------------------------

The PostgreSQL code base is developped and maintained by a swarm of developers
from all over the world, communicating constantly together... If you subscribed
to the psql-hackers mailing list, you know that it's humanly impossible to
follow every discussions on a daily basis. However when it comes about Graphic
Tools, the situation is completely opposite. There's little discussion happening
between the different developpers. Every project seems like an oasis in the
middle of the desert.

__What can we do about it ?__ The developer meeting itself is a first answer to
that. We'll try to organize a second one in 2018 (probably in Ottawa during
PGCon) and hopefully we can have these kind of gathering on a regular basis in
different places... We can also create other spaces to talk : a forum, an IRC
channels, etc. we'll see what we need along the way...  Right now a dedicated 
mailing list is probably the best starting point.




We lack basic UI/UX design skills 
-------------------------------------------------------------------------------

No offense to anyone. Developing a database management tool is hard and most of
the GUI developpers I know are primarily DBAs. Very often, they created their 
software for their own need and they're doing their best to build a nice interface. 
But we're in competion against some big companies (Oracle, Microsoft and others).  
FIXME

__What can we do about it ?__ If you're a PostgreSQL company, please hire UI/UX
experts and make them work with the various existing projects. If you're a
developper with some skills in design, please share your experience and your
knowledge.  

We don't know our users
-------------------------------------------------------------------------------

Among the projects presented during the meeting, very few add conducted a proper
[User Research](https://en.wikipedia.org/wiki/User_Research). In fact, most of
them didn't clearly define what is their user base. Again I'm not blaming the 
developpers here, but FIXME

FIXME

__What can we do about it ?__ Again UI/UX design 




We need support from the whole Postgres community
-------------------------------------------------------------------------------

Yes `psql` is great. Most advanced PostgreSQL admins use it as their primary
tool. But very few people **started** using Postgres with `psql`. Graphic Tools
are the entrypoints of our community. We should never forget that at one point
we were all just beginners and that today's beginners are the future of our
community.


__What can we do about it ?__  This one is tough. It may require us to get rid
of a huge culture bias. We need to realize that improving graphic tools is
important for everyone, even for users that use exclusively the commande line.  
There's no magic formula for that, and this culture shift will probably take a
while but hopefully we'll get there.


Now What ? 
-------------------------------------------------------------------------------





