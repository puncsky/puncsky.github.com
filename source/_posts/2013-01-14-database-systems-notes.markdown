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
		- simple and popular but not memory-efficient, not scale
			- easy to debug
		- IBM DB2, PostgreSQL, Oracle
	2. thread per DBMS worker
		- a single multi-threaded process hosts all the DBMS worker activity.
		- a dispatcher listens for new DBMS client connections.
		- scale but have multi-threaded programming challenges.
		- IBM DB2, SQL Server, MySQL. Informix, Sybase
		- lighter-weight and more portable than OS threads
		- BUT
			- blocking operations are problematic
			- replicating code already existent in the OS (context switching, thread state management, scheduling)
			- long term code maintenance obligation
			- super hard to debug 
	3. process pool
		-  A central process holds all DBMS client connections and, as each SQL request comes in from a client, the request is given to one of the processes in the process pool. 
		- simpler than 2 and more memory-efficient than 1
	- Conclusions:
		- poor OS thread support in the 70s lead to 1 ,2
		- **Modern implementations tend to go with thread pool model**
		- Legacy databases are somewhat stuck with whatever model they currently have
		- New models
- Shared data and process boundaries
	- full DBMS worker independence & isolation not possible, for on same shared DB
		- thread per DBMS worker: same address space
		- process per DBMS worker & process pool: shared memory
	- How to transfer results from DB to client? 
		- Various buffers are used:
			1. Disk I/O buffers
				- *DB I/O requests: The Buffer Pool.* 
					- 1: on heap in shared address space / 
					- 2&3: in shared memory in the other 2 models
				- *Log I/O requests: The Log Tail.* in memory queue that periodically flushed to the log disk(s) in FIFO order (generally, a separate process is responsible for this)
					- 1: a heap-resident data structure
					- 2&3: a) a separate process b) in shared memory and flushed by all threads/processes
					-  commit transaction flush. A transaction cannot be reported as successfully committed until a commit log record is flushed to the log device. 
			2. Client Communication buffers
				- pull model: clients issue SQL FETCH request repeatedly
				- Client communications socket as a queue for the tuples
				- *Lock table* is shared by all DBMS workers, used by Lock Manager to implement DB locking semantics
- admission control
	- DBMS thrashing: the result of 1)memory pressure 2)contention for locks
	- Admission control: does not accept new work unless sufficient DBMS resources are available.
	- to achieve _graceful degradation_: latency & throughput
	1. The dispatcher process ensures that the number of client connections is kept below a threshold.
	2. Execution admission controller
		- aided by information from the query optimizer
			
### 3. Parallel Architecture: Processes and Memory Coordination  

- shared memory
	- The main challenge is to modify the query execution layers to take advantage of the ability to parallelize a single query across multiple CPUs.
- shared nothing
	- horizontal data partitioning: each system in the cluster stores only a portion of the data.
	- Good partitioning places a significant burden on DBA
	- partial failure
- shared disk
	- Oracle RAC and DB2 for zSeries SYS- PLEX
	- increasing popularity of SAN (Storage Area Networks)
	- no partition of data -> lower cost of administration than shared-nothing
	- distributed buffer pools <- distributed lock manager & cache-coherency protocol
	
	
- NUMA
- DBMS threads and multi-processors
- standard practice