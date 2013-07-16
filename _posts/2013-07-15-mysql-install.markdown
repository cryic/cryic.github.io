---
layout: post
title:  "mysql install"
date:   2013-07-15
categories: mysql installation 
---

Attempting to install the mysql2 gem, I ran into some problems.

I was getting an error saying that `mysql.h is missing`.

I couldn't seem to google up an answer until finding [Kim Scott's blog
post][blog-link].

The solution comes down to editing the `mysql_config` file to remove
compliation flags.

Extremely helpful and could happen whenever setting up
environments on a new machine.

[blog-link]: http://www.randomactsofsentience.com/2013/05/gem-install-mysql2-missing-mysqlh-on-os.html
