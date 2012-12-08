---
layout: post
title: "HadoopDB: an architectural hybrid of MapReduce and DBMS technologies for analytical workloads (VLDB '09)"
date: 2012-11-20 16:20
comments: true
categories: Paper Review
---

[Yale DB](http://db.cs.yale.edu/hadoopdb/)

### 1. Problem

Large analytical data management (OLAP) with commodity clusters

### 2. Challenges

- Performance and efficiency
	- hadoop
		- is not for structured data analysis
		+ scale well
		+ open source, without cost
- scalability, fault-tolerance, and flexibility
    - Previous parallel db fit into tens of nodes, not thousand of nodes.
	- not scale well for failures, heterogeneous machines, performance untested

### 3. Solutions

- MapReduce (Hadoop) + parallel db (or single-node DBs) = HadoopDB<!--more-->
	- Translation layer: Hive 
	- Communication layer: Hadoop
	- Database layer: PostgreSQL (or MySQL, ... JDBC)
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
		intercept normal Hive flow in
        1. update MetaStore before query execution
		2. between query plan generation and MR jobs
          retrieve fields, determine partition keys

          traverse DAG bottom-up

        only support filter, select, aggregation
			
- Extend performance [23] with fault tolerance and heterogeneous node exp

    TODO: modify the current task scheduler, connect not straggler node but the replicas.

    Exp on EC2, Hadoop, HadoopDB, Vertica, DBMS-X

### 4. Conclusion

- HadoopDB's performance < parallel db for
	- PostgreSQL (not column-store, not compression)
	- Hadoop and Hive are young
+ performance, heterogeneous environ., fault tolerance, flexibility



### Related Readings

[23] A Comparison of Approaches to Large Scale Data Analysis

[6] Scope [11] Hive [24] C-store [4] What is the right way to measure scale?



