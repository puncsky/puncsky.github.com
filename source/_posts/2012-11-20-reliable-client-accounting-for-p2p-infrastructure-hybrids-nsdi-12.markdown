---
layout: post
title: "Reliable Client Accounting for P2P-Infrastructure Hybrids (NSDI '12)"
date: 2012-11-20 16:16
comments: true
categories: Paper Review
---
[MPI-SWS](http://www.mpi-sws.org/~paditya/), UPenn, Duke, Akamai, [paper](http://www.mpi-sws.org/~paditya/papers/rca-nsdi2012.pdf), [slides](https://www.usenix.org/sites/default/files/conference/protected-files/nsdi12_reliable_client_accounting_for_p2p-infrastructure_hybrids.pdf), [video](https://www.usenix.org/conference/nsdi12/reliable-client-accounting-hybrid-content-distribution-networks)

1. Problems
----

Hybrid (Servers with assisting peers) designs in CDNs -> malicious clients can cause significant accounting inaccuracies.

2. Challenges
----

Infrastructure can not control P2P communications by malicious clients (peers). Even if infrastructure provides signed metadata and fallback (so content can not be mishandled by peers), there are still:

- Affect service quality
- Misreport P2P transfers

For example, inflation attack occurred for a fake download report.

3. Solutions
----

Reliable Client Accounting (RCA)
<!--more-->
Clients keep logs of network activity and upload them to the infrastructure periodically. The infrastructure collects logs, verifies them, and isolate suspicious nodes.

### 1. Record client activities reliably

Tamper evident logging: log recording sending/receiving history forms hash chains. Every massages contains a signature (authenticator) from its sender (O(# of messages)). -> Later, RCA only records authenticators for clients (O(# of pairs)). 

### 2. Identify misbehaving/suspicious clients

By accounting the logs, infrastructure can find clients unilaterally claim fake downloads. 

As to malicious client software:

What if bad clients do not follow the above steps (e.g. do not keep logs and serve bad content)? Simplify NetSession protocol to a state machine. Rules are manually set to identify bad logs not following the them. 

What if many clients collude to cheat? It is difficult in practice (infrastructure assigns peers), but can still be found by statistical checks.

As to malicious users:

What if a user repeatedly downloading content to drive up demand? (can be amplified by Sybil attack) Statistical checks, too.

### 3. Handle misbehavior without affecting service quality

Blacklist and quarantine bad clients.

4. Conclusions
----

Since infrastructure cannot observe P2P communication, accounting is vulnerable to malicious clients (peers) in a hybrid design of CDN, e.g. inflation attacks. So the authors advance RCA to 1) keep tamper-evident logs and set rules for fixed state machines 2) perform statistical analysis on collected logs. Malicious clients can be detected and isolated effectively by RCA. The evaluation with real world Akamai NetSession shows that overhead is reasonable <= 0.5%.