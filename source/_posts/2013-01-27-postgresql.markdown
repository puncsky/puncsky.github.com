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

Chapter 27 from your textbook. Focus on 27.1, 27.4 until (and including) 27.4.1.4, 27.5, 27.6, and 27.7. The majority of our discussion in class will focus on 27.4.


27.1 Introduction 1123
27.2 User Interfaces 1124
27.3 SQL Variations and Extensions 1126
27.4 Transaction Management in PostgreSQL 1137
27.5 Storage and Indexing 1146 27.6 Query Processing and Optimization 1151
27.7 System Architecture 1154 
Bibliographical Notes 1155