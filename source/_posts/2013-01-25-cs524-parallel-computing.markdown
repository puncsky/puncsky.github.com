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
	