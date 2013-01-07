---
layout: post
title: "Three Papers Review about Deterministic Parallelism"
date: 2012-11-20 16:10
comments: true
categories: paper_review
---

[Yale Dedis](http://dedis.cs.yale.edu/2010/det/)

### 1. Efficient System-Enforced Deterministic Parallelism (OSDI’10)

Parallelism introduces 1) non-determinism and 2) data races (heisenbugs). Determinism means that a given input always produces the same output. In other words, input alone determines the output, regardless of extrinsic events such as the OS’s thread scheduling. 

To achieve determinism,<!--more--> Determinator, an OS offering a programming model that is naturally and pervasively deterministic, is introduced. Its private workspace model solves data races in the first place, and the model is deterministic at all levels of abstraction. Like a version control system, this model gives a thread a private replica of all the state and the thread operate within its private state but could not interact directly with other threads until reconcile. At this time, write-write races become conflicts and determinator would throw an exception when main thread joins them.

In terms of implementation, determinator takes an arbitrarily deep hierarchy of spaces, consisting of CPU register state and private virtual memory. The space is like a single-threaded process but different from the concepts of process and thread. Interaction is allowed only for the space’s parent and child spaces via put, get and return three system calls. It could be applied to multiprocessor/multicore system and also multiple nodes in a homogeneous cluster. In high level abstractions, it emulates traditional fork/exec/wait APIs, and shared state abstractions with no physical state sharing, which involves Distributed Shared Memory techniques. Its runtime maintains a complete file system replica in the address space of each process, with the copy-on-write mechanism. The runtime treats I/O as a special case of file system synchronization for the reason of space hierarchy.

Determinator is written in C with small assembly fragments. PIOS is a subset of it, and the former is partly derived from MIT’s JOS.

Since determinator is a primitive proof-of-concepts prototype, it inevitably has some limitations:

1. A restrictive space hierarchy -> a performance bottleneck for I/O-bound applications     AND no support for non-hierarchical synchronization, queue, future
2. Limited address space -> limited file system size
3. No focus on file system -> no persistent storage
4. Inefficient cross-node communication: no prefetching or other optimization, Eternet only.


### 2. Workspace Consistency: A Programming Model for Shared Memory Parallelism (WoDet ‘11)

To address the 1st limitation, Workspace Consistency: A Programming Model for Shared Memory Parallelism extends WC from OSDI’10 version of hierarchical structure to a more generalized non-hierarchical structure. It supports non-hierarchical synchronization patterns (dynamic producer/consumer graphs and inter-thread queues), besides hierarchical synchronization patterns such as fork/join and barrier. WC highlights matched release/acquire pairs, adding three constrains to Release Consistency (RC):

1. Release/acquire pair is unique.
2. One thread’s writes is not visible to another thread’s read until sync.
3. Data races are handled by throwing a runtime exception or other deterministic ways.

Two prototypes: one on Linux, one on Determinator, both supporting only hierarchical synchronization patterns.

This generalized WC extends Determinator’s virtual memory system to support a Single Producer Multiple Consumer (SPMC) shared memory primitive – SPMC channels. The implementation adds 2 optional arguments to existing Put/Get system calls. Determinator maps the virtual memory ranges of the producer and all consumers to the same physical memory. Consumer would be blocked until the producer fixes the page.It is suitable for applications demanding pipeline parallelism or "all-to-all" communication. 

Full WC model atop SPMC extension is not yet implemented.


### 3. Deterministic OpenMP for Race-Free Parallelism (HotPar ‘11)

DOMP is a variant of OMP based on the WC model. It keeps parallel, loop, sections, barrier, excludes atomic, critical, flush, generalizes reduction as reduction (function : list), and extends the sections with pipeline clause.


