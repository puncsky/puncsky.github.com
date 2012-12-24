---
layout: post
title: "CLRS 3: Selected"
date: 2012-12-22 19:12
comments: true
categories: 
---

## VII Selected Topics
Introduction 769
### 27 Multithreaded Algorithms 772

- serial algorithms
- parallel algorithms
	- No agreement on a single architectural model for parallel computers
		- shared memory (multicore)
			- ***static threads*** -> complex schedule and load-balance -> concurrency platforms
				- **dynamic multithreading platform**
					- 2 features: 1. nested parallelism. Subroutine can be spawned. (divide-and-conquer) 2. parallel loops.
					- 3 keywords: parallel, spawn, sync.
					- Platforms: Cilk, Cilk++, OpenMP, Task Parallel Library, Threading Building Blocks, Intel TBB.
		- distributed memory

#### 27.1 The basics of dynamic multithreading 

- The keyword **spawn** does not say, however, that a procedure must execute concurrently with its spawned children, only that it _may_. It is up to a scheduler at run time.
- A procedure cannot safely use the values returned by its spawned children until after it executes a sync statement

算Fibonacci，recursion 的时候，spawn一个，自己算一个，sync，对这两个结果求和。

##### A model for multithreaded execution

- Linear speedup: T1/Tp = Θ(P); Perfect linear speedup: T1/Tp = P

27.2 Multithreaded matrix multiplication 792 
27.3 Multithreaded merge sort 797
### 28 Matrix Operations 813

- numerical stability: limited precision
- numerically unstable

28.1 Solving systems of linear equations 813
28.2 Inverting matrices 827
28.3 Symmetric positive-definite matrices and least-squares approximation 832
### 29 Linear Programming 843
29.1 Standard and slack forms 850
29.2 Formulating problems as linear programs 859 29.3 The simplex algorithm 864
29.4 Duality 879
29.5 The initial basic feasible solution 886
### 30 Polynomials and the FFT 898

- Fast Fourier transform, or FFT, can reduce the time to multiply polynomials to Θ(n lg n).

30.1 Representing polynomials 900 30.2 The DFT and FFT 906
30.3 Efficient FFT implementations 915
### 31 Number-Theoretic Algorithms 926

- Cryptographic schemes based on large prime numbers.

31.1 Elementary number-theoretic notions 927 
31.2 Greatest common divisor 933
31.3 Modular arithmetic 939
31.4 Solving modular linear equations 946 
31.5 The Chinese remainder theorem 950
31.6 Powers of an element 954
31.7 The RSA public-key cryptosystem 958
31.8 Primality testing 965
31.9 Integer factorization 975

### 32 String Matching 985

- T[n]: text
- P[m]: pattern (m <= n)
- s: shift - pattern P occurs beginning at position _s+1_ in text T
	- valid shift
	- invalid shift

#### 32.1 The naive string-matching algorithm 988 

O((n-m+1)*m)

#### 32.2 The Rabin-Karp algorithm 990

- 把字符串转换成一位一位的数字，用霍纳法则
- Θ(m) preprocessing, worst-case Θ((n-m+1)*m)
- used to detect plagiarism 

#### 32.3 String matching with finite automata 995
#### 32.4 The Knuth-Morris-Pratt algorithm 1002
### 33 Computational Geometry 1014
33.1 Line-segment properties 1015
33.2 Determining whether any pair of segments intersects 1021 33.3 Finding the convex hull 1029
33.4 Finding the closest pair of points 1039
### 34 NP-Completeness 1048

- Problems which can be solved by polynomial-time algorithms are **tractable/easy**. Otherwise, **intractable/hard**
- Three classes of problems:
	1. P.	solvable in O(n^k)
	2. NP.	verifiable in polynomial time.
	3. NPC.	NP-complete 
		- If any NP-complete problem can be solved in polynomial time, then every problem in NP has a polynomial-time algorithm.
		- Many CS scientists believe NP-complete problems are intractable.
34.1 Polynomial time 1053
34.2 Polynomial-time verification 1061
34.3 NP-completeness and reducibility 1067 34.4 NP-completeness proofs 1078
34.5 NP-complete problems 1086
### 35 Approximation Algorithms 1106
35.1 The vertex-cover problem 1108
35.2 The traveling-salesman problem 1111
35.3 The set-covering problem 1117
35.4 Randomization and linear programming 1123 35.5 The subset-sum problem 1128