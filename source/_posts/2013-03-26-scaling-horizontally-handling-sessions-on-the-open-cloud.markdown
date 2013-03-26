---
layout: post
title: "Scaling Horizontally: Handling Sessions on the Open Cloud"
date: 2013-03-26 11:52
comments: false
author: Hart Hoover
categories: 
- Five Pillars
- Cloud Servers
- REST
---
Wayne Walls wrote a great article on the Rackspace Blog around [horizontal scale](http://www.rackspace.com/blog/pillars-of-cloudiness-no-3-scaling-horizontally/), a pillar of cloud application design. When designing applications in the cloud, typically you need more than one server performing specific tasks.

{% tweet https://twitter.com/DEVOPS_BORAT/status/274366602252804096 align='center' %}

These groups of servers or roles or tiers are sometimes load balanced or exist as a pool of servers polling a message queue for work. In this article, I want to focus on the former - load balanced servers.

->![](a/2013-03-28-scaling-horizontal/arch1.png)<-

In traditional application design in the example above, you would need to do something to manage sessions across these three servers. The most popular ways are typically:

* Place a cookie on the end user's system
* Store sessions in Memcached
* Store sessions in some sort of database

With the advent of "[Do Not Track](http://www.zdnet.com/googles-chrome-finally-embraces-do-not-track-but-with-a-warning-7000007022/)" browser plugins (or DNT being built into the browser itself) the first option isn't going to work for everyone anymore.

Storing sessions in Memcached? It isn't [recommended](https://code.google.com/p/memcached/wiki/NewProgrammingFAQ#Why_is_memcached_not_recommended_for_sessions?_Everyone_does_it!) by the Memcached developers:

> Why is memcached not recommended for sessions? Everyone does it!

> If a session disappears, often the user is logged out. If a portion of a cache disappears, either due to a hardware crash or a simple software upgrade, it should not cause your users noticable pain.

Storing sessions in a database means extra load on that database - something you don't want to have in the cloud. It also could mean lag time while replication occurs to any number of slaves (in the case of a relational database) or nodes (in the case of NoSQL). A less-than-ideal service experience occurs when your end user refreshes a page and hits a node without their session data. At the very least it means they get logged out of your service, at most it could mean their shopping cart gets reset.

So what can be done about sessions? We need a session to live SOMEWHERE and we need it to be trusted.