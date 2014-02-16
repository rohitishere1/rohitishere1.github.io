---
layout: post
title: "Mysql Logging"
description: "Mysql logging for debugging and slow query logging and identification"
tags: ["Mysql","database","logging","slow query logging"]
---

Mysql logging and slow query indentification
===================================================

Mysql allows you to log slow and erroneous queries. This obviously has a mild impact on performance on systems with average load. On systems with huge load, this has to be taken based on call. This only requires to change some config changes described below.
First of all, I am describing this process for mysql version 5.5.31. For other versions I have usually found the configuration similar, so must not be a headache to figure out.

This config changes needs to be done on `my.cnf`, usually at the path `/etc/mysql/my.cnf`.
Once you have opened, you will see by default the mysql error logging on, and path is also specified, if not, you might be required to do as below, remove the `#` to uncomment the line and give a path for log file as done below

{% highlight bash %}
	log_error = /var/log/mysql/error.log
{% endhighlight %}
for slow queries, there would be a commented chunk as below
{% highlight bash %}
#log_slow_queries       = /var/log/mysql/mysql-slow.log
#long_query_time = 2
#log-queries-not-using-indexes
{% endhighlight %}
This needs to be uncommented to come into action:

**log_slow_queries:** acts as a flag to log slow queries and provides path to log.

**long_query_time:** by defaults value is 10 and minimum value is 1 (in seconds, unsigned integer values only).

**log-queries-not-using-indexes:** flag which enables logging of look up queries without indexes.

now, if you are in some kind of debugging, migration, monitoring or audit, then you can enable general query log and all the queries you run get logged. But one must know that this is a performance killer and all options and requirements should be weighed out before going for it.
{% highlight bash %}
#general_log_file        = /var/log/mysql/mysql.log
#general_log             = 1
{% endhighlight %}
uncomment the above lines for them this to come into action.
