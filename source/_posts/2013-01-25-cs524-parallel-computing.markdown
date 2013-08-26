---
layout: post
title: "CS524 Parallel Computing"
date: 2013-01-25 20:51
comments: true
categories: parallel
published: false
---

## 01 Overview 

- Faster clock speeds -> more power consumption -> more heat and higher temperature -> Unreliability. So we can’t continue to make individual processors faster simply by reducing size and increasing clock speeds. **So we have to exploit the density by building many processors (cores) per chip running at a lower clock speed: parallelism**
- [Linpack Benchmark](http://www.top500.org/project/linpack/) -> top500 list
- types of parallel computers
	1. Distributed Memory Multicomputer (MPI, Linda, PVM, ...)
	2. Shared Memory Multiprocessor (Pthreads, OpenMP, HPF)
	3. GPU - SIMD
	4. Hybrid
- Course content
	- MPI, pthreads, OpenMP, CUDA
	- Types of parallel computations (focus on science/engineering)
	- Performance measurement, tuning, and debugging
	- Parallel computer architecture
	
## 02 Some Basic Background

- The von Neumann Architecture: CPU(ALU registers + Control registers)--interconnect--main memory(address|contents)
- fork and join 
- von Neumann Bottleneck exists in the *interconnect between CPU and main memory*: 
	- Programmers want unlimited amounts of memory with low latency BUT DRAM bandwidth is insufficient(only 6%)
	- Solution: Use multi-level hierarchies of memory:
		- L1 0.5ns, L2 7ns, L3 10ns~20ns, Mem 100ns
	- Principle of locality: access _near locations_, _consecutively(spatial locality)_, and _soon(temporal locality)_.
	- General Cache Organizations 
		- S = 2^S sets
		- E = 2^e lines per set
		- B = 2^b bytes per cache block
- Read throughput
	- Stride (distance between ints)
	- Total working set (total size of the array in bytes) [Note: Number of elements read ≈ TWS/(sizeof(int)*stride) ]
- Use miss rate instead of hit rate
- Memory mountain 

## 03 Intro to Parallelism

- Types of parallelism
	- instruction level parallelism
		- multiple issue (e.g. superscalar)
		- pipelining 
	- multicore parallelism
	- multinode parallelism
- Flynn's Taxonomy
	- SISD(single instruction stream, single data stream)
		- classic von Neumann
	- SIMD
		- vector processors (GPUs)
		- Parallelism achieved by dividing data among functional units such as arithmetic logic units (ALUs)
		- Applies the same instruction to multiple data items (vectors)
		- Sometimes combined with pipelining
		- DRAWBACKS: same instruction, must be synchronously
	- MISD 
		- uncommon, sometimes used for fault-tolerance
	- MIMD
		- multicore/multinode parallel machines
- Speedup and efficiency
	- e = speedup / nodes
- Amdahl's Law: 
	- Smax -> 1/f   (f = serial/total)
	- Bad news: Emax -> 0
- Gustafson's Law
	- Gustafson’s Law assumes a fixed time for the serial section (Ks)
	- whether to increase the problem size?
		- Gustafson's Law: If we keep the efficiency fixed by increasing the problem size at the same rate as we increase the number of processes/threads, the problem is **weakly scalable**.
		- If we increase the number of processes/threads and keep the efficiency fixed without increasing problem size, the problem is **strongly scalable**.
- t\_parallel = t\_computation + t\_overhead
	- Overhead time is a duration that starts with the release of a shared resource and ends with the receipt of that resource. Ideally, the duration of Overhead time is very short because it reduces the time a thread has to wait to acquire a resource.

## 04 Message-Passing Concepts

- Message routing between nodes typically done by ***daemon processes*** installed on the nodes that form the “virtual machine”.
- tools
	- PVM Parallel Virtual Machine, 1980s
	- MPI
- Message-Passing Software APIs
	- process management
	- communication
- MPMD (multiple program, multiple data model)
- SPMD (basic MPI way)
- MPI\_Send(buf, count, datatype, dest, tag, comm)
- MPI\_Recv(buf, count, datatype, src, tag, comm, status)

## 05 Message-Passing Using MPI

- Semantics of Blocking Communication
	- Blocking
		– MPI_Send: 
			- Returns only when it is safe to modify buffer
			- Completes later (or possibly never)
		– MPI_Recv
			- Completes when the buffer contains message data
			- Returns only when the buffer contains the message
	- Locally-ordered
		- Global sequence is not ensured because ordering is a partial ordering, and it IS NOT transitive!!
	- Progress
		- Possible race condition
	- Fairness
		- no guarantees that all processes get services.
- Send/Recv Arguments 
	– MPI Types
		Message-passing has 3 phases:
			1. Copy from send buffer to assemble a message
				- MPI type in call must match the language type in buffer
			2. Message transfer from send daemon to recv daemon
				- Datatypes provided to send and recv must match
			3. Disassemble message, copying into recv buffer
				- MPI type in call must match the language type in buffer
	– Tags 
		- Integer with upper bound `MPI_TAG_UB`
		- `MPI_ANY_TAG`
		- Tags Crucial for Libraries
	– Status
		- C: `MPI_Status` C++: `MPI::Status` 
		- Source: `status.MPI_SOURCE` `Get_source()`
		- Tag: `status.MPI_TAG` `Get_tag()`
		- Error: `status.MPI_ERROR` `Get_error()`
		- Count: `MPI_GET_count(*status, datatype, *count)` `Get_count(datatype)`
- Semantics of Non-Blocking Communication
	- Locally non-blocking
		- MPI\_Isend and MPI\_Irecv **return immediately**
			- “Return” is not the same as “complete”
			- Requires care! Don’t modify message buffer too soon!
			- Completion is independent of whether user checks for completion
		- “Request Object” used to identify each posted operation 
		- User should check non-blocking operations for completion
			- Blocking Checks:
				- `MPI_WAIT...`
			– Non-blocking Checks:
				- `MPI_TEST...`
		- Posted operations are locally-ordered
			- No local system buffering in most implementations
			- “Non-overtaking” property extends to non-blocking Sends
		- Progress (same)
		- Fairness (same)
- Additional Topics
	- Polling for Messages 
		- `MPI_Probe`, `MPI_Iprobe' test whether there are messages with matching envelop (source, tag, comm)
		- `MPI_Cancel()`
	- P2P Communication Modes
		- Standard (MPI_Send, MPI_Isend)
			- Send completes when caller’s message buffer is reusable
			- May or may not use system-level buffering (implementation choice) – Non-local (completion may depend on receiving process)
		- Buffered (MPI_Bsend, MPI_Ibsend)
			- Like Standard, but must use caller-provided buffer (1 per process) – Local (completes when data is in local caller-provided buffer)
		- Synchronous (MPI_Ssend, MPI_Issend)
			- Like Standard, but Recv will have started when Send completes – Implies synchronization between source and destination
		- Ready (MPI_Rsend, MPI_Irsend)
			- Like Standard, but call asserts that matching Recv has been posted. (Otherwise, operation is erroneous and behavior is undefined) 
		- MPI_Recv, MPI_Irecv are the only Recv operations
		
## 06 Collective Operations in MPI

- Principal collective operations:
	- `MPI_Barrier()` Synchronizes processes in communicator; by blocking each one until all call MPI_Barrier
	
	- `MPI_Bcast()` Broadcast from one root process to all processes
	- `MPI_Scatter()` Root scatters buffer in parts to group of processes
	- `MPI_Gather()` Root gathers data from group of processes
	- `MPI_Allgather()` Like Gather, except all processes receive data
	- `MPI_Alltoall()` Like simultaneous scatters from all processes
	
	- `MPI_Reduce()` Root receives 1 value combining values on all procs
	- `MPI_Allreduce()` Like Reduce, except all procs receive the result 
	- `MPI_Reduce_scatter()` Combine values and scatter results
	- `MPI_Scan()` Compute prefix reductions of data on processes
