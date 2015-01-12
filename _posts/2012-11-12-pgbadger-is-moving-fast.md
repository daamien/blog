---
layout: post
title: "pgBadger is moving fast"
description: ""
category: english
tags: [PostgreSQL,pgBadger]
---

When the pgBadger project started 6 month ago, our primary goal was to
build a better performance tool for PostgreSQL. We also wanted it to be
an open and dynamic project, built with the help of the Postgres
community. So far the result is beyond our expectations : we've received
dozens of user requests, bug reports and contributions.

<!-- More -->

Thanks to this community support, the project goes at a very fast pace !
Today Gilles Darold released [pgBadger
2.2](http://www.dalibo.org/pgbadger_2.2_released). This new version
comes with a bunch of exciting new features:

-   [Tsung](http://tsung.erlang-projects.org) support : This means you
    can now create benchmark scenarios based on the queries contained in
    your PostgreSQL logs
-   New parameters : For instance with `--exclude-user` you can now
    ignore a user in the generated reports. With `--select-only` you
    generate a report about the read queries...
-   Faster CSV log parsing (thanks to David Fetter)
-   Improved log format detection
-   And many bugfixes....

And there's more come ! We're currently working on two major
enhancements :

-   **Multi-threading is on it's way** : The last limitations have been
    removed and there's now a clear path to implement parallel parsing
    of the log files. pgBadger is already quite fast, but this will make
    it even faster. Especially on servers with many CPUs.

-   **HTML5 Overhaul** : For now pgBadger reports look pretty much like
    the ones produces by
    [pgFouine](http://pgfouine.projects.pgfoundry.org) : i.e. it's just
    basic HTML aimed for a desktop screen. This was fine in 2005, but
    now we need a responsive design ! Reports must be more interactive
    and readable on any devices. This is a tough job and as DBAs we have
    limited design skills :-) So we decided to hire a professional web
    design company to help us with : the French company [art is
    code](http://www.artiscode.net/) will work with us to build a
    smarter report.

Of course, we will involve the community while working on these two
major improvements. If you want to participate and just keep in touch
with the project, please join the [pgBadger mailing
list](https://listes.dalibo.com/cgi-bin/mailman/listinfo/pgbadger). In a
few week, we will submit the first draft of the new report design.

In a nutshell : If you don't know what pgBadger is, give it a try ! If
you already use it, please upgrade to version 2.2 ! And if you've
already installed v2.2, stay tuned because version 3 is coming fast !

By the way, we're not alone... The whole postgres community seems to be
buzzing with brand new performance tools ! Over the past few weeks, at
least 3 interesting projects popped up :

-   [datascope](http://datascope.herokuapp.com/) is a nice tool to
    visualize pg\_stat\_statements data
-   [pg\_stat\_plans](http://www.2ndquadrant.com/en/pg_stat_plans/)
    allows you to lists SQL statements by execution plan, not just by
    query.
-   [pgOtter](https://github.com/omniti-labs/pgOtter) is yet another
    small mammal trying to parse your PostgreSQL logs ;-)

At a personal level, I'm quite happy to see this happening and I bet
this is just the beginning of a great move of the community toward
better user-friendly tools for the PostgreSQL user base.

