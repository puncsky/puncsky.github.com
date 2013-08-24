---
layout: post
title: "Towards Statistical Queries over Distributed Private User Data (NSDI '12)"
date: 2012-11-20 16:13
comments: true
categories: paper_review
---

From [MPI-SWS, Germany](https://sites.google.com/site/ruichuanc/), [paper](https://73aab115-a-62cb3a1a-s-sites.googlegroups.com/site/ruichuanc/pddp-nsdi12.pdf?attachauth=ANoY7cqTHmW8qn2UrCjxk0u-eafRsp77w2XtpG9QdnY9nOnDKkELrYiH-RrOI2ILFnQUv6gt-oz_ek1DA8q7TptjvCEWWWpT02huRCgNYXW-bUNQwjJjM0DLN7tiJzOKD509vt1JhOZ_fHQ_rijDX5Dhh3Bx1pdZotJp7mDbCw0yrcSTYEfbXAuzkZK2zDxfRKYzZXww-dESgY9wquSilSiX3ZrPYrATOg%3D%3D&attredirects=0) and [slides](https://www.usenix.org/sites/default/files/conference/protected-files/pddp-talk-nsdi12.pdf)

1. Problem
---

To protect user privacy in distributed systems from leaking by statistical queries.

2. Challenges
---

The most direct solutions are

1. to **anonymize + add noise** to *user data*.

	[-] utility, de-anonymize

2. differential privacy. add noise to *answer of queries*. 

    [-] scale, churn tolerance, malicious client

3. Solution 
---

PDDP: Practical Distributed Differential Privacy. 
<!--more-->
- Binary answer in bucket. The query result should not be distorted by the client arbitrarily.
- Blind noise addition. The Malicious should not be trusted. Private data are controlled by its user only.

### 3.1 Assumption

Clients and analysts are potentially malicious. Proxy is HbC (honest but curious) and should not have access to noise-free result.

### 3.2 Work Flow

1. Query Initialization(Analyst -> Proxy)

2. Query Forwarding (Proxy -> Client)

3. Client Respond (Client -> Proxy)

   answers are encrypted with the analyst's public key. 

4. Differential Private Noise Addition.

   collaborative coin generation with a GM cryptosystem. Unbiased proxy flip encrypted coins from clients randomly and thus transform them into unbiased ones. Coins serve as DP noises. 

5. Noisy Answers to Analyst ( Proxy -> Analyst)

### 3.3 Implementation and Deployment

600+ Client = Firefox add-on + SQLite

Proxy = Tomcat web service + MySQL

Analyst = Java program

4. Conclusion
===

The authors achieve scalable, churn-tolerant user privacy against malicious analyst and clients by

1. making a trade off between utility and privacy. (differential privacy)
2. introduce distributed system to traditionally centralized differential privacy environment. (distributed)
