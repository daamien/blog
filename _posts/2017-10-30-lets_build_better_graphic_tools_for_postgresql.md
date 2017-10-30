---
title: "Lets build better graphic tools for PostgreSQL" 
date: 2017-10-30 08:17:36
layout: post
description: "A quick report from the PostgreSQL Graphical Tools Developers Meeting"
author: damien clochard
twitter_id: @daamien
github_id: daamien
category: english
tags: [PostgreSQL, GUI]
---

The very first [PostgreSQL Graphic Tools Developer
Meeting](https://wiki.postgresql.org/wiki/PostgreSQL_Graphic_Tools_Developper_Meeting_2017) 
happened last week during [PostgreSQL Conference Europe 2017](https://2017.pgconf.eu) 
in Warsaw. The meeting gathered 16 developpers repesenting 14 software (
[KEXI](http://www.kexi-project.org/), 
[Postico](https://eggerapps.at/postico/),
[Postgres.app](https://postgresapp.com/), 
[pgAdmin](https://www.pgadmin.org/), 
[DBeaver](https://dbeaver.jkiss.org/), 
[pgCluu](http://pgcluu.darold.net/)
[SQL Tabs](https://www.sqltabs.com/), 
[pg_view](https://github.com/zalando/pg_view), 
[bg_mon](https://github.com/CyberDem0n/bg_mon), 
[pgBadger](http://dalibo.github.io/pgbadger/), 
[pgFormatter](https://github.com/darold/pgFormatter),
Postgres Professional Manager (not released yet), 
[temBoard](http://temboard.io/),
[PoWA](http://dalibo.github.io/powa/), 
[pgDevOps](https://www.openscg.com/bigsql/pgdevops/), 
) 
and 10 different organizations (
[EnterpriseDB](https://www.enterprisedb.com/), 
[OpenSCG](https://www.openscg.com/), 
[PostgresPro](https://postgrespro.com/), 
[EggerApps](https://eggerapps.at/), 
[Crunchy Data](https://www.crunchydata.com/), 
[Dalibo](https://dalibo.com/),
[KDE](http://kde.org/), 
[2nd Quadrant](https://www.2ndquadrant.com),
[Rider Soft](https://dbeaver.com/contacts/),
[Zalando](https://www.zalando.fr/)
)

Here's my personal report from the meeting, you can also find a more general summary on the PostgreSQL wiki: 

<https://wiki.postgresql.org/wiki/PostgreSQL_Graphic_Tools_Developper_Meeting_2017>

<!--MORE-->

[![PostgreSQL Graphic Tools Developer Meeting](https://raw.githubusercontent.com/daamien/blog/gh-pages/_images/pgtdm.jpg)](https://wiki.postgresql.org/wiki/PostgreSQL_Graphic_Tools_Developper_Meeting_2017)

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
projects such as [PhpPgAmin](http://phppgadmin.sourceforge.net/) and the ones we 
forgot (apologies to 
[Datagrip](https://www.jetbrains.com/datagrip/) 
and [Elephant Shed](https://github.com/credativ/elephant-shed) !).

Now let's compare this to other competitors : Oracle DBAs have `Cloud Control` or
`TOAD`. MySQL users have `PhpMyAdmin` or `Workbench`. For MSSQL
Server, the `Management Studio` is pretty much your only choice.

Of course open source is all about diversity, but at some point too much choice is
becoming a problem. New PostgreSQL users are getting really confused by this
profusion of tools and the comminty workforce is split in separate projects. 
All in all it is slowing down the PostgreSQL adoption.

__What can we do about it ?__ We need to raise awareness of the community and
make a better at promoting the existing solutions. The message must be clear :
if you want to build a PostgreSQL administration tool, please fork pgAdmin or
another one... But don't start a new one from scratch....


We don't talk to each other
-------------------------------------------------------------------------------

The PostgreSQL code base is developped and maintained by a swarm of developers
from all over the world, communicating constantly together... If you subscribed
to the psql-hackers mailing list, you know that it's humanly impossible to
follow every discussions on a daily basis. However when it comes about graphic
tools, the situation is completely opposite. There's little discussion happening
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

No offense to anyone here. Developing a database management tool is hard and most of
the GUI developpers I know are primarily DBAs. Very often, they created their 
software for their own need and they're doing their best to build a nice 
interface. But we're in competition against some big companies (Oracle, 
Microsoft and others) and they investing a lot in this area. Instread of tyring
to copycat compititing like `Oracle Cloud Control` or `PhpMyAdmin`, we need to 
innovate and improve the design our tools.

__What can we do about it ?__ If you're a PostgreSQL company, please hire UI/UX
experts and make them work with the various existing projects. If you're a
developper with some skills in design, please share your experience and your
knowledge.  

We don't know our users
-------------------------------------------------------------------------------

Among the projects presented during the meeting, very few add conducted proper
[User Research](https://en.wikipedia.org/wiki/User_Research). In fact, most of
them didn't clearly define what is their user base. Again I'm not blaming the 
developpers here, but if we want to improve the PostgreSQL users experience, we 
need a better understanding of who our users are, what they want and how they 
use our software.  

Waiting passively for user feedbacks and bug reports is not enough !

__What can we do about it ?__ Again there are multiple tools and methods in the 
UI/UX design domain that we can learn, adapt and share : 
[personas](https://en.wikipedia.org/wiki/Persona_(user_experience)), 
[user interviews](https://www.interaction-design.org/literature/article/how-to-conduct-user-interviews), 
[User Experience Evaluation](https://en.wikipedia.org/wiki/User_experience_evaluation),
etc.




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


[![PostgreSQL Graphic Tools Developer Meeting](https://raw.githubusercontent.com/daamien/blog/gh-pages/_images/pgtdm_group_photo_2017.jpg)](https://wiki.postgresql.org/wiki/PostgreSQL_Graphic_Tools_Developper_Meeting_2017)

Now What ? 
-------------------------------------------------------------------------------

This first meeting left me with many questions but also with great hopes. I am 
pretty sure we will see big improvements in this area in the forthcoming years.

In fact, PostgreSQL is now so feature rich that we've now reached a point where 
the new challenges are outside the database core itslef. Improving our 
Graphic tools is one of these future challenges and if we can gather only 10% 
of the community's collective intelligence on this matter, we're going to make 
great things !





