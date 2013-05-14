---
layout: post
title: "Transaction Management in the R* Distributed Databases Management System (TODS)"
date: 2013-04-03 03:03
comments: true
categories: 
published: false
---

From [IBM Almaden](http://dl.acm.org/citation.cfm?id=7266)

### Comments

Although 2-phrase commit is a standard commit protocol, its cost is very expensive. This paper introduces presumed abort (PA) and presumed commit (PC) for optimization. However, in my opinion, it lacks valid experimental results and performance analysis to examine in which cases it is better or not.

The presumed commit protocol expects most transactions to commit and thus ACKs for COMMITS could be avoided. One noteworthy thing is that if both root and subordinate are in prepared state but the root crashes before voting, the root would forget the transaction on recovery. The solution is that coordinator could record subordinates first by force-writing a collecting record in the beginning of the first phrase.

Deadlock detector (DD) exists at every site, building wait-for graphs locally. The implementation of Potential Global Deadlock Cycles (PGDC, dependency lists of transactions) is not quite clear. The paper says PGDCs are sent around in the direction of the real/potential deadlock cycle. However, I am a little confused about how they are issued and whom they are issued from.

### 1. Problem

### 2. Challenges

### 3. Solution

### 4. Conclusion

