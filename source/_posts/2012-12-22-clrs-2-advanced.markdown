---
layout: post
title: "CLRS 2: Advanced"
date: 2013-01-08 19:11
comments: true
categories: algorithm
---

## IV Advanced Design and Analysis Techniques Introduction 357

- Three important but more sophisticated techniques used in designing and analyzing efficient algorithms:
	1. dynamic programming
		- used to optimize problems in which we make a set of choices to get optimal solution
		- store the solution to each such subproblem in case it should reappear.
	2. greedy algorithms
		- used to optimize problems in which we make a set of choices to get optimal solution
		- make each choice in a locally optimal manner. e.g. 买东西找钱算法
		- matroid theory as a mathematical basis
	3. amortized analysis
		- used to analyze certain algorithms that perform a sequence of similar operations.
		- Bound the cost of the entire sequence s.t. although some operations might be expensive, many others might be cheap. 
- We have covered before:
	1. Divide-and-conquer
	2. Randomization
	3. Recurrence

### 15 Dynamic Programming 359

- Divide-and-conquer: subproblems are disjoint
- Dynamic Programming: subproblems overlap. 
	- solve subproblems just once and save it in a table, avoiding recomputing
	- applied to **optimization problems**: find ***a*** solution with the optimal (min/max) value
	- four steps:
		1. Characterize the structure of an optimal solution.
		2. Recursively define the value of an optimal solution.
		3. Compute the value of an optimal solution, typically in a bottom-up fashion (get the result)
		4. Construct an optimal solution from computed information (get how to compute the result)
	
#### 15.1 Rod cutting 360

- Input: a rod of length _n_ inches. a table of prices _pi_ for i = 1,2,...,n
- Output: max revenue rn

#### 15.2 Matrix-chain multiplication 370
#### 15.3 Elements of dynamic programming 378 
#### 15.4 Longest common subsequence 390 
#### 15.5 Optimal binary search trees 397

### 16 Greedy Algorithms 414
#### 16.1 An activity-selection problem 415
#### 16.2 Elements of the greedy strategy 423 
#### 16.3 Huffman codes 428
#### 16.4 Matroids and greedy methods 437
#### 16.5 A task-scheduling problem as a matroid 443
### 17 Amortized Analysis 451
#### 17.1 Aggregate analysis 452 
#### 17.2 The accounting method 456 
#### 17.3 The potential method 459 
#### 17.4 Dynamic tables 463

## V Advanced Data Structures Introduction 481
### 18 B-Trees 484
18.1 Definition of B-trees 488
18.2 Basic operations on B-trees 491 18.3 Deleting a key from a B-tree 499
### 19 Fibonacci Heaps 505
19.1 Structure of Fibonacci heaps 507
19.2 Mergeable-heap operations 510
19.3 Decreasing a key and deleting a node 518 19.4 Bounding the maximum degree 523
### 20 van Emde Boas Trees 531
20.1 Preliminary approaches 532 20.2 A recursive structure 536 20.3 The van Emde Boas tree 545
### 21 Data Structures for Disjoint Sets 561
21.1 Disjoint-set operations 561
21.2 Linked-list representation of disjoint sets 564 21.3 Disjoint-set forests 568
21.4 Analysis of union by rank with path compression 573


## VI Graph Algorithms
￼Introduction 587
### 22 Elementary Graph Algorithms 589 
#### 22.1 Representations of graphs 589 

- Sparse graphs: |E| is much less than |V|^2
	- adjacency list Θ(V+E)
- Dense graphs: |E| is close to |V|^2 or when we need to be able to tell quickly if there is an edge connecting two given vertices.
	- adjacency matrices Θ(V^2)
- edge (u,v) has attribute ƒ
	
#### 22.2 Breadth-first search 594

- BFS with queue
	- *u.color* (WHITE for ones having not been to, GRAY for ones in the queue, BLACK for ones having been dequeued and enqueued their white neighbors), *u.π* the predecessor, *u.d* distance

``` BFS pseudo code

	BFS(G, s)
		initialize vertices except source
		initialize the source vertex
		enqueue the source
		while (queue) {
			dequeue u
			for each white neigbor 
				mark attributes
				enqueue
			u.color = BLACK
		}

```

#### 22.3 Depth-first search

- DFS with recursion (stack)
	-  timestamps are integers between 1 and 2 jV j, since there is one discovery event and one finishing event for each of the jV j vertices. 

#### 22.4 Topological sort 612

- topological sort **with DFS**
- DAG(directed acyclic graph)
- a **topological sort** of a graph as an ordering of its vertices along a horizontal line so that all directed edges go from left to right.
- to indicate precedence among events.
- TODO 有多个入度为0的

``` Toplogical sort pseudo code

	toplogicalSort(G) {
		call DFS(G) to compute finishing times v.f for each vertex v
		as each vertex is finished, insert it onto the front of a linked list
		return the linked list of vertices
	}
```

#### 22.5 Strongly connected components 615

- decomposing a directed graph into its strongly connected components **with DFS**

### 23 Minimum Spanning Trees 624
23.1 Growing a minimum spanning tree 625 23.2 The algorithms of Kruskal and Prim 631
### 24 Single-Source Shortest Paths 643
#### 24.1 The Bellman-Ford algorithm 651
24.2 Single-source shortest paths in directed acyclic graphs 
#### 24.3 Dijkstra’s algorithm 658

- faster than Bellmen-Ford algorithm


24.4 Difference constraints and shortest paths 664
24.5 Proofs of shortest-paths properties 671
655
### 25 All-Pairs Shortest Paths 684
25.1 Shortest paths and matrix multiplication 25.2 The Floyd-Warshall algorithm 693 25.3 Johnson’s algorithm for sparse graphs
686 700
### 26 Maximum Flow 708
#### 26.1 Flow networks 709

- **Flow networks**. Let directed graph G = (V, E) be a flow network with a capacity function c. Let s be the source of the network, and let t be the sink. A flow in G is a real-valued function f: V*V -> R that satisfies:
	1. **Capacity constraint**. For all u,v in V, we require 0 <= f(u,v) <= c(u,v)
	2. **Flow conservation**. The rate at which material enters a ver- tex must equal the rate at which it leaves the vertex.
- Value |f| of a flow f is the total flow out of the source minus the flow into the source. **maximum-flow problem**: we are given a flow network G with source s and sink t, and we wish to find a flow of maximum value.


#### 26.2 The Ford-Fulkerson method 714 

- dependent on
	1. residual networks
	2. augmenting path
	3. cuts
- We repeatedly augment the flow until the residual network has no more augmenting paths.

26.3 Maximum bipartite matching 732
26.4 Push-relabel algorithms 736
26.5 The relabel-to-front algorithm 748

