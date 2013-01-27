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
		- important data to remember (TODO zoo)
	- Principle of locality: access _near locations_, _consecutively(spatial locality)_, and _soon(temporal locality)_.
	- General Cache Organizations 
		- S = 2^S sets
		- E = 2^e lines per set
		- B = 2^b bytes per cache block
- Read throughput
	- Stride (distance between ints)
	- Total working set (total size of the array in bytes) [Note: Number of elements read ≈ TWS/(sizeof(int)*stride) ]
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
		- DRAWBACKS: same instruction, synchronously
	- MISD 
		- uncommon, sometimes used for fault-tolerance
	- MIMD
		- multicore/multinode parallel machines
- Speedup and efficiency
	- e = speedup / nodes
- Amdahl's Law: Smax = 1/f
- Gustafson's Law
	- Gustafson’s Law assumes a fixed time for the serial section (Ks)
	- Gustafson's Law: If we keep the efficiency fixed by increasing the problem size at the same rate as we increase the number of processes/threads, the problem is **weakly scalable**.
	- If we increase the number of processes/threads and keep the efficiency fixed without increasing problem size, the problem is **strongly scalable**.
- t\_p = t\_computation + t\_overhead
	- Overhead time is a duration that starts with the release of a shared resource and ends with the receipt of that resource. Ideally, the duration of Overhead time is very short because it reduces the time a thread has to wait to acquire a resource.

## Message-Passing Concepts

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