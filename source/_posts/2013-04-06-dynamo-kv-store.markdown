---
layout: post
title: "Dynamo: Amazon's Highly Available Key-value Store (SOSP'07)"
date: 2013-04-06 14:16
comments: true
categories: paper_review
---

From [Amazon](http://www.allthingsdistributed.com/2007/10/amazons_dynamo.html)

### Comments

Dynamo successfully builds a highly available and scalable data store, which sacrifice consistency under certain failure scenarios but is eventually consistent. The design is huge and involves a huge number of details. However, the paper could not specify all of them. Some meaningful parts are missing: what are the supporting techniques used in Dynamo? How it is compared to other existing KV distributed store systems? Are there any possible extensions or future works?

This paper assumes that security concerns could be ignored for its internal use. Theoretically, security problems is still possible. At least, how can the administrator detect these attacks? e.g. sybil attacks.

The author tried three partition schemes. The latter versions decouple partitioning and partition placement. The placement is changeable at run time. Consequently, the strategy 3 has a better efficiency. And the system recovers faster and it is easier to archive. However, the key space is partitioned equally into Q partitions, and every node assume Q/S tokens per node. However, the strategy 3 in Figure 7 is a little confusing. In my opinion, the author should specify that the span each node is responsible for is SIMILAR IN SIZE to each other, instead of exactly EQUAL IN SIZE to each other. 

### 1. Problem

- How to build a highly available key-value storage system (which will be applied to productions with demanding applications)?
- Assumptions:
	1. Query model: Simple KV store with read/write operations on small objects (<= 1 MB).
	2. ACID properties: Weak consistency and no isolation guarantees for high availability.
	3. Efficiency: Commodity machines. Stringent latency requirements specified by SLAs.
		- **SLA** (service level agreements): a client/server agreement on clear bounds
			- SLA should NOT be stated in terms of mean/median response time (Some custom with longer history may require more processing and thus the performance may be ignored.)
			- Dynamo: **99.9%** of the distribution based on a cost-benefit analysis.
			- Can be customized. 
	4. No security concerns: because of internal use.

### 2. Challenges

- How to achieve a strongly consistent data access interface? 
	- Data replication algorithms. Strong consistency and high data availability cannot be achieved simultaneously. Traditionally, consistency comes first.
- How to increase availability on failure-prone commodity clusters? 
	- Optimistic replication techniques -> conflicting changes to be resolved -> **WHEN** and **WHO**?
		- **WHEN** during writes or reads?
			- Traditionally, conflict is resolved during writes and keep the read complexity simple.
			- Dynamo: rejecting writes -> poor customer experience: writes should never be rejected. **(always-writable)**
		- **WHO** data store or the application?
			- data store: ONLY last write wins
			- the application: more flexible
- Others
	- Incremental scalability
	- Symmetry (P2P)
	- Decentralization
	- Heterogeneity

### 3. Solution

- Key ideas
	- Sacrifice consistency under certain failure scenarios but is eventually-consistent
	- object versioning
	- application-assisted conflict resolution

#### 3.1 System Architecture

##### Summary 

<table border="1">
 <tbody><tr>
  <td>
  <p align="center"><b>Problem</b></p>
  </td>
  <td>
  <p align="center"><b>Technique</b></p>
  </td>
  <td>
  <p align="center"><b>Advantage</b></p>
  </td>
 </tr>
<tr>
  <td>
   <p align="center">Partitioning</p>
  </td>
  <td>
  <p align="center">Consistent Hashing</p>
  </td>
  <td>
  <p align="center">Incremental Scalability</p>
  </td>
 </tr>
 <tr>
  <td>
  <p align="center">High Availability for writes</p>
  </td>
  <td>
  <p align="center">
  Vector clocks with reconciliation during reads</p>
  </td>
  <td>
  <p align="center">Version size is decoupled from update rates.</p>
  </td>
 </tr>
 <tr>
  <td><p align="center">Handling temporary failures</p>
  </td>
  <td>
  <p align="center">Sloppy Quorum and hinted handoff</p>
  </td>
  <td>
  <p align="center">
  Provides high availability and durability guarantee
  when some of the replicas are not available.</p>
  </td>
 </tr>
 <tr>
  <td>
  <p align="center">
  Recovering from permanent failures</p>
  </td>
  <td>
  <p align="center">Anti-entropy using Merkle trees</p>
  </td>
  <td>
  <p align="center">Synchronizes divergent replicas in the background.</p>
  </td>
 </tr>
 <tr>
  <td>
  <p align="center">Membership and failure detection</p>
  </td>
  <td>
  <p align="center">Gossip-based membership protocol and failure
  detection.</p>
  </td>
  <td>
  <p align="center">Preserves symmetry and avoids having a centralized
  registry for storing membership and node liveness information.</p>
  </td>
 </tr>
</tbody>
</table>


- System Interface
	- *[object/list of objects with context] get(key)*
	- *put(key, context, object)	// context encodes system metadata about the object*
	- context and object are opaque
	- MD5
- Dynamic Partition
	- Introduce *virtual nodes* as a variant of consistent hashing.
- Replication
	- Every **coordinator** node stores data locally and replicates them at the following *N-1* nodes in the ring.
	- The N physical machines forms a **preference list**.
- Data Versioning
- (Successful) Execution of put() and get()
	- HTTP request
	- How to select a node? two ways:
		1. **load balancer** route it to any random node in the ring. If the node is not in the top N of the requested key's preference list, the request is forwarded to the first among the top N nodes in the preference list.
		2. **partition-aware client library** route it directly to the coordinator
	- R: min number of nodes that must participate in a successful read.
	- W: min number of nodes that must participate in a successful write.
	- *R+W>N* -> quorum-like system 
	- How to define a successful write?
		- >= W-1 nodes respond 
- (Unsuccessful) Execution of put() and get()
	- 
- Membership

##### Implementation

##### Lessons Learned

- Dynamo's configurations
	- Business logic specific reconciliation
	- Timestamp based reconciliation
	- High performance read engine
- *N, R, W*
	- N -> durability of each object
	- W, R -> availability, durability, and consistency
	- Dynamo: (3,2,2)
- Performance or Durability?
	- Durability can be sacrificed to lower the 99.9% latency by a factor of 5, in which object buffers will be periodically written to the disk by a *writer thread*.
- Load balance
	1. T random tokens per node and partition by token value.
		- Schemes for partitioning and partition placement are intertwined. 
		a) adding a new node costs a lot but has a low priority (should not affect the customer performance) -> slow
		b) key ranges change and Merkle trees (hash trees) need re-calculation.
		c) randomness -> hard to archive the entire key spaces.
	2. T random tokens per node and equal sized partitions.
	3. Q/S tokens per node, equal-sized partitions.

### 4. Conclusion

- “Always writable” asynchronous replication
- Update may not propagate all replicas
- put() creates a new and immutable version of data
- get() may return multiple versions of data
- Reconcile divergent versions
- When: during reads
- Who: system itself (syntactic reconciliation) client application (semantic reconciliation)