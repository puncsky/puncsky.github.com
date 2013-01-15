---
layout: post
title: "Database Systems Notes"
date: 2013-01-14 20:23
comments: true
categories: 
published: false
---

## DB Internals

### 1. Introduction

#### 1.1 Relational System: The Life of a Query

- RDBMS has 5 main components:
	1. Client Communications Manager
		- two-tier: client-server
		- three-tier: add a proxy tier
		- four-tier: add an application server
	2. Process Manager
		- admission control: when to process?
	3. Relational Query Processor
	4. Transactional Storage Manager
		- ensure ACID (atomicity, consistency, isolation, durability)
	5. Shared Components and Utilities 

### 2. Process Models

- Uniprocessors and Lightweight Threads, 3 models:
	1. process per DBMS worker
		- simple and popular but not memory-efficient
		- IBM DB2, PostgreSQL, Oracle
	2. thread per DBMS worker
		- a single multi-threaded process hosts all the DBMS worker activity.
		- a dispatcher listens for new DBMS client connections.
		- scale but have multi-threaded programming challenges.
		- IBM DB2, SQL Server, MySQL. Informix, Sybase
	3. process pool
		-  A central process holds all DBMS client connections and, as each SQL request comes in from a client, the request is given to one of the processes in the process pool. 
		- simpler than 1 and more memory-efficient than 2
- Shared data and process boundaries
	- How to transfer results? 
		- Various buffers are used:
			1. Disk I/O buffers
			2. 
### 3. Parallel Architecture: Processes and Memory Coordination  
