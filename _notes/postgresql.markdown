---
title: Notes about PostgreSQL
layout: page
permalink: /notes/postgresql/
---

[PostgreSQL](http://www.postgresql.org/) is an open source object-realtional database management system (ORDBMS) which available in many platforms.

## Better way to backup database

{% highlight bash %}
$ pg_dump --no-privileges --no-owner --no-reconnect database_name > output.sql
{% endhighlight %}
