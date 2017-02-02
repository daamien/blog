---
date: 2017-02-02 22:17:36 
layout: post
title: "Temboard Quick Start "
description: "Temboard is a new management tool for PostgreSQL"
category: english
tags: [PostgreSQL, GUI, control, management]
---

The next challenge for the PostgreSQL is to provide feature rich and well-designed graphic tools to manage several instances. This is a domain where we can make a lot of progress and [Temboard](https://github.com/dalibo/temboard) is one of the current projects (among others) that aims to provide a nice management tool for PostgreSQL.

<!-- More -->

![Temboard : PostgreSQL Remote Control](http://temboard.readthedocs.io/en/latest/temboard.png)

Since we're talking about a graphic tool, let's have a quick look ! Here's 3 steps to deploy a testing
environment composed of 3 PostgreSQL instances and a central Temboard server.  

1- [Install docker](https://docs.docker.com/engine/installation/) 
   and [Install docker-compose](https://docs.docker.com/compose/install/)

2- Fetch our pre-configured compose file and launch it

```
wget https://raw.githubusercontent.com/dalibo/docker/master/temboard/docker-compose.yml
docker-compose up
```

3- Go to <https://127.0.0.1:8888>

* Connect to the Temboard server with `admin / admin`
* Connect to each Postgres instance with `alice / alice`
* The temboard server is listening to the local LAN

You should see something like this :

![Temboard PostgreSQL Dashboard](https://github.com/dalibo/temboard/raw/master/doc/demo_dashboard.gif)

That's all for today ! I'll make another post in a few days to explain the features of Temboard such as the live dashboard, the `postgresql.conf` versioning, how to kill a transaction, etc.

Meanwhile I want take some time to test this tool please send us an email at [contact@dalibo.com](mailto:contact@dalibo.com) and let us know what you think about it,  what features are missing for you, etc. Temboard is still in early development stage and we plan to add more plugins in the forthcoming month ! 


''EDIT: Typos''
