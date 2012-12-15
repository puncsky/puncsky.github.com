---
layout: post
title: "Fast Crash Recovery in RAMCloud (SOSP'11)"
date: 2012-12-13 12:22
comments: true
categories: Paper Review
---

[Stanford](http://www.stanford.edu/~ouster/cgi-bin/home.php), [paper](http://www.stanford.edu/~ouster/cgi-bin/papers/ramcloud-recovery.pdf), [slides](https://ramcloud.stanford.edu/wiki/display/ramcloud/RAMCloud+Presentations), [video](http://www.youtube.com/watch?v=RE3CmdEGv4M)

### 1. Problem

RAMCloud is a storage system helping developers manage large-scale DRAM storage, driven by the fact that large-scale Web applications need to cache large data in DRAM. (For example, Facebook cached 150TB DRAM in _memcached_ out of 200TB of disk storage.)

In order to keep a high level of durability and availability without impacting system performance, RAMCloud has only one single copy of data in DRAM. The problem here is how to recover from crash within 1s~2s.

### 2. Challenges

- **Durability**. RAM is lack of durability. Data is unavailable on crashed nodes.
- **Availability**. How to recover as soon as possible?
	- Synchronous disk writes too slow
- **Large scale**. 10,000 nodes, 100TB to 1PB

### 3. Solution

- For durability, keep a _pervasive log structure_. Even RAM is a log.
- For availability, employ data parallelism and pipelining while backup data are distributed across a large number of secondary storage devices.

Architecture Overview

(Up to 100,000) application servers are connected to (up to 10,000) master/backup storage servers. Each storage server contains a master and a backup. Masters expose RAM as key-value store, while backups store data from other Masters. A central coordinator manages the server pool and tablet configuration.

When a master receives a write requests, it updates its in-memory log and forwards the new data to several backups, which buffer the data in their memory. Master maintains a hash table to record locations of data objects. The data is eventually written to disk or flash in large batches. **Backups must use an auxiliary power source to ensure that buffers can be written to stable storage after a power failure.**

When a master crashes, its in-memory log and hash table will be lost, but log data stored on disk on backups will survive. 

How to recover? The crashed and restarted master replays log data into RAM and reconstruct the hash table.

How to recover with scale? Partition and scatter log data to more backups randomly. So backup data can be read in parallel.

How about the network bandwidth bottleneck of NIC on recovery master? More recovery masters are used and each master's data are also partitioned. Before recovery, backups receive a partition list specifying which master should what part of data should be sent to.

**What if one backup is slow and make the whole process slow?** The master chooses candidate Backups randomly and selects the best one according to its locality, disk information, etc.

Performance: recovers 35GB to RAM in 1.6s using 60 nodes.

### 4. Conclusion

The authors use the log-structured storage instead of synchronous disk write to preserve durability. And the availability is achieved via data parallelism. The design harness the scale well and has exceptional performance. 