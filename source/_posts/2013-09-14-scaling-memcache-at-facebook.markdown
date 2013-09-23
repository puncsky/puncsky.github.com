---
layout: post
title: "Call-of-Duty Weekend Reading: Scaling Memcache at Facebook"
date: 2013-09-14 00:13
comments: true
categories: paper_review
published: false
---

### 1. Problem

- Construct a distributed KV store processing billions of requests per second
- Assumptions
	- reading-intensive > write
	- heterogeneous sources: MySQL, HDFS, backend services..

### 2. Challenges

- goals to achieve
	- general use for user and operation. optimization with limited scopes are not considered
	- always-readable
- keywords: replication, consistency, optional complexity, fault-tolerance

### 3. Solution

#### 3.1 In a Cluster: Latency and Load
#### 3.2 In a Region: Replication
#### 3.3 Across Regions: Consistency
#### 3.4 Single Server Improvements
#### 3.5 Memcache Workload

### 4. Conclusion






