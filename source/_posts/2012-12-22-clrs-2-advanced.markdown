---
layout: post
title: "CLRS 2: Advanced"
date: 2012-12-22 19:11
comments: true
categories: 
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
22.1 Representations of graphs 589 22.2 Breadth-first search 594
22.3 Depth-first search
22.4 Topological sort 612
22.5 Strongly connected components 615
### 23 Minimum Spanning Trees 624
23.1 Growing a minimum spanning tree 625 23.2 The algorithms of Kruskal and Prim 631
### 24 Single-Source Shortest Paths 643
24.1 The Bellman-Ford algorithm 651
24.2 Single-source shortest paths in directed acyclic graphs 24.3 Dijkstra’s algorithm 658
24.4 Difference constraints and shortest paths 664
24.5 Proofs of shortest-paths properties 671
655
### 25 All-Pairs Shortest Paths 684
25.1 Shortest paths and matrix multiplication 25.2 The Floyd-Warshall algorithm 693 25.3 Johnson’s algorithm for sparse graphs
686 700
### 26 Maximum Flow 708
26.1 Flow networks 709
26.2 The Ford-Fulkerson method 714 26.3 Maximum bipartite matching 732
? 26.4 Push-relabel algorithms 736
? 26.5 The relabel-to-front algorithm 748

