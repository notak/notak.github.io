---
title: "MySQL on Ubuntu 16.04"
summary: "It has bad defaults and leaks memory like a sieve. Setting up a dev server shouldn't be this hard."
layout: default
---

If you are trying to run MySQL on an even remotely small server running Ubuntu 16.04 you might be experiencing some issues. Say for example you are trying to run a database as a small part of a web application on a test server with a gigabyte of memory. It's fair to expect that small part to run in a couple of hundred megabytes at most right? With a default install on 16.04 you'll be lucky if it runs at all. There are couple of reasons for this. One fairly long-standing and easy to fix, and one obscure, and hopefully just an update away from resolution.

## MySQL versions from 5.6 onwards are just plain greedy

The first is that MySQL versions from 5.6 onwards (on Ubuntu at least) are just plain greedy with their default installs. This manifests in a couple of key ways:

*Performance schema is enabled by default*. This is excellent news for dedicated servers with a decent amount of ram. At the cost of just a couple of hundred megabytes you get great information about how your server is performing, which is just what you need when you have 4gb of RAM dedicated to your database and want to tune as much performance as possible out of it. Ironically the performance schema output can even tell you what happened to all the RAM. If you are running a development instance, or any kind of shared workload instance, you will want to include `performance_schema = off` in the `[mysqld]` section of your config. 

*InnoDB buffer pool is too big*. Actually, if you are running a dedicated instance InnoDB buffer pool is far too small, since the recommendation is around 70-80% of available memory, but for development purposes it is crazily large. I have no idea how they managed to come up with a value that suits nobody, but they sure hit the sweet spot. On a 512M machine the InnoDB buffer pool is set to grab 128M. I'm not sure whether this is a static default or it goes for a quarter of available memory, but either way it's more than you have to spare. You'll want to add `innodb_buffer_pool_size=32M` (or thereabouts) to the `[mysqld]` section of your config.

Overall this is crazy, and exactly where the Debian and Ubuntu packagers could be adding value. They should take the approach that the vast majority of default installs are shared instances and optimize accordingly. If you are running a large dedicated instance and couldn't be bothered to tune it all then your performance is going to suck anyway, and you are incredibly unlikely to actually look at the output of the performance schema. The alternative would be to ask in the install process how much system memory you want to dedicate to the MySQL server and adjust accordingly, though to be honest I think that would be a waste of time.

## There is a memory leak

Versions 5.7.8 thru 5.7.13 of MySQL have a memory issue. It looks like more of a crazy over-allocation than a leak as such, and in practice it gets worse for a few hours and then stabilizes, but the number it stabilizes at is pretty high. Unfortunately Ubuntu 16.04 runs 5.7.12 and is therefore affected. The solution is probably to jump a couple of minor versions in an update, but it's not wise to bank on this happening.

So what's the solution? Depends on your needs really. You could just add the 15.10 repositories to get access to MySQL 5.6 packages and then downgrade, or look for a PPA and manually install. I've chosen to just follow the workaround suggested in the bug report for now, but memory usage for me is hovering around the 350-400M point, so whether you choose to do that or not really depends on your needs. 

## Why this matters

It matters because it's wasted half my day, on something incredibly minor. Making it this difficult to use a common component on Ubuntu is going to drive me away from Ubuntu. And throwing in random bugs is going to drive me away from MySQL. A lot of blogs on the topic recommend just switching to MariaDB, and the only reason I don't already use Postgres is because I find it more fiddly. It's probably not wise to expect that inertia to last. 