---
layout: post
title: "Call-of-Duty Weekend Reading: Scaling Memcache at Facebook (NSDI '13)"
date: 2013-09-23 07:37
comments: true
categories: paper_review
published: true
---

I felt a little under the weather last week, which prevented me from being productive. After a serious consideration over the last month (the first month I undertook a full-time job), I set up a new schedule for myself -- get up earlier so that I can have at least four hours in the morning for my own work without any distractions. Hope I can persist with the amazing plan.

At the same time, as I have planned long ago, a new paper-reading project will be launched. Day after day, I find myself now soaked in technical details. It is good for improving my engineering skills, but not so good for seeing the big picture. Moreover, reading papers may be the best way to collect excellent ideas. At least, it is much better than reading newspapers, SNS feeds, and disappointing books (e.g. [淘宝技术这十年](http://book.douban.com/subject/24335672/) ).

This paper comes from Facebook ( [video](https://www.usenix.org/conference/nsdi13/scaling-memcache-facebook), [paper](https://www.usenix.org/system/files/conference/nsdi13/nsdi13-final170_update.pdf), [slides](https://www.usenix.org/sites/default/files/conference/protected-files/nishtala_nsdi13_slides.pdf) ), and introduces how the KV store evolves at Facebook in these years.

Months ago, I posted a similar paper review, [Dynamo: Amazon's Highly Available Key-value Store (SOSP'07)](http://www.puncsky.com/blog/2013/04/06/dynamo-kv-store/). Different from Amazon, Facebook built the KV store on the basis of existing open-source memcached (memcached refers to the source code, while memcache refers to the store system). One thing I notice to be in common is that both of them push complexity into the client whenever possible, because a lighter data store is more flexible. Both of them add additional marks along with the cached content for future processing.