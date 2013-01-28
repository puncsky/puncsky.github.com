---
layout: post
title: "PostgreSQL"
date: 2013-01-27 08:18
comments: true
categories: 
published: false
---

## Chapter 44. Overview of PostgreSQL Internals	

### 44.1. The Path of a Query

1. connect
2. parse and create a query tree
3. rewrite system looks for any rules
4. query plan to executor
5. executor

### 44.2. How Connections are Established

- process per user
	- master `postgres` listens at a specified TCP/IP port
	- spawns a new server process
	
### 44.3. The Parser Stage

