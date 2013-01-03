---
layout: post
title: "Quincy: Fair Scheduling for Distributed Computing Clusters (SOSP '09)"
date: 2012-11-20 16:08
comments: true
categories: paper_review
---
[Microsoft](http://research.microsoft.com/en-us/people/misard/), [paper](http://www.sigops.org/sosp/sosp09/papers/isard-sosp09.pdf), [video](http://www.sigops.org/sosp/sosp09/videos/19_michael_isard.mov), [slides](http://www.sigops.org/sosp/sosp09/slides/quincy/QuincyTestPage.html)

1. Problem
----

How to achieve fair scheduling? In other words, a guy who gets up early and performs huge tasks on a cluster should not always monopolize most computing resource, and someone else's assignments should not be ignored. Otherwise, it is unfair for all the cluster users.

**Fair sharing** of the cluster resources

Job x takes *t* seconds, when running exclusively on the cluster. When the cluster has *J* jobs, x should take <= *Jt* seconds.

*N* computers and *J* jobs: each job gets at least N/J computers.

2. Challenges
----

Traditionally,
<!--more-->
MPI Model, tasks are in a pipeline and then assigned to a part of cluster. 

- If one node is down, all the processes should be killed and the user have to start at a new checkpoint.
- Coarse grain allocation. Allocation is static. 
- Off cluster data strage, e.g. SAN

Dryad MapReduce Model

- No communication between slaves. 
- No fine grain sharing for resource competence. Many Idle nodes.

**Fine-grain sharing model** 

- Multiplex all computers in cluster between jobs.
- When a task completes, computer may be assigned to another job.
- Jobs uses *N/J* computers at a time but set in use varies over lifetime.

3. Solution
----

The solution is intended for data-intensive computing with locality. There is no SAN. However, data locality conflicts with fairness. So they present Quincy: a new, graph-based framework for cluster scheduling under a fine grain cluster resource-sharing model with locality constrains. 2 basic ideas:

1. sub-optimal assignment of a job's tasks.
2. kill running tasks to free resources
  
### 3.1 Queue-based Scheduling

Architecture: A core switch (CS) manages rack switches (RC). A rack switch (RC) manages computers (C). For example, C1, C2 and C3 in a rack have their own queues, and share a same rack queue. R1 R2 managed by the same core switch have their own queues above, and share a same core queue. Every time a task X is finished, X will be deleted from all the queues, no matter what hierarchy it is in.

So how to get fairness?

### 3.2 Flow-based Scheduling

Simplify a scheduling problem to a matching problem. 

- each task is either scheduled or unscheduled.
- can assign a cost to any matching
- fairness constrains number of tasks that are scheduled

How to minimize matching cost while still maintaining fairness?

Min-cost network flow.

There are U (unscheduled nodes), X (cluster aggregator nodes), R (rack aggregator nodes), C (computing nodes). In addition to queue-based scheduling, the edges connecting tasks to these nodes have weights showing the cost of the matching. The capacities on the outgoing edge of job *j*'s unscheduled node *Uj* control the number of running tasks that the job will be allocated.

4. Conclusion
----

The authors advance a new fair schedule modeling for Dryad/MapReduce/Hadoop by min-cost network flow, achieving much better performance and effectiveness than traditional ways.
