---
layout: post
title: "HadoopDB (VLDB'09)"
date: 2013-01-27 15:50
comments: true
categories: paper_review
---

[Yale DB](http://db.cs.yale.edu/hadoopdb/hadoopdb.html)

### Comments

Two ways to improve HadoopDB

1. Integrate the file system for local database with HDFS. In the design of HadoopDB, the file system and the database are separated, which means that the more space one part is preallocated and the less space the other part will occupy. This design is not scalable because the system administrator has to partition the space by hand. We could investigate a way to let the database run on the HDFS directly. The database's file descriptor can be adapted to HDFS directly. Or, even better, we can add an extra virtualized layer between the database and the HDFS.
2. Set up a better scheduler. It is mentioned in the paper that the original version of hadoop fair scheduler is not so good. It is queue-based and locally greedy and simply does not consider the big picture much. "Quincy: Fair Scheduling for Distributed Computing Clusters" advances a flow-based algorithm, which has a very good performance by increasing the throughput up to 40%.

<!--more -->

###1. Problem

Large analytical data management with commodity clusters

### 2. Challenges

- Performance and efficiency
	- hadoop 
		- is not for structured data analysis
    	- scale well
    	- open source, without cost
- scalability, fault-tolerance, and flexibility
	- Previous parallel db fit into tens of nodes, not thousand of nodes.
		- not scale well for failures, heterogeneous machines, performance untested
		
### 3. Solutions

- MapReduce (Hadoop) + parallel db (or single-node DBs) = HadoopDB
	Translation layer: Hive 
	Communication layer: Hadoop
	Database layer: PostgreSQL (or MySQL, ... JDBC)
- Components
	1. Database Connector
		- Interface among dbs on nodes
		- extends InputFormat class -> InputFormat Implementation lib
		- JDBC-complaint
	2. Catalog
		- metainformation as XML = connection para + metadata
	3. Data Loader
		- Global Hasher: MR, repartition raw data from HDFS to NO. of nodes
		- Local Hasher: copy from HDFS, repartition the partition into chunks
		- Better load balance than Hadoop
    4. SQL - MR - SQL (SMS) Planner
		- extend Hive for
        	+ open and low cost
        	- each table stored separately in HDFS, low performance in multi-table trans. 
		- intercept normal Hive flow in
        	1. update MetaStore before query execution
        	2. between query plan generation and MR jobs
            	retrieve fields, determine partition keys
            	traverse DAG bottom-up
			only support filter, select, aggregation
- Extend performance [23] with fault tolerance and heterogeneous node exp
	- TODO: modify the current task scheduler, connect not straggler node but the replicas.
- Exp on EC2
	- Hadoop
    - HadoopDB
    - Vertica
    - DBMS-X

### 4. Conclusion

- HadoopDB's performance < parallel db for
	PostgreSQL (not column-store, not compression)
	Hadoop and Hive are young
+ performance, heterogeneous environ., fault tolerance, flexibility

### Related Readings

[23] A Comparison of Approaches to Large Scale Data Analysis
[6] Scope [11] Hive [24] C-store [4] What is the right way to measure scale?