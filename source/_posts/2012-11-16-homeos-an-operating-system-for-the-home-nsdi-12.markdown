---
layout: post
title: "HomeOS: An Operating System for the Home (NSDI '12)"
date: 2012-11-16 22:06
comments: true
categories: 
---
[IBM](http://researcher.watson.ibm.com/researcher/view.php?person=us-ckd), [MS](http://research.microsoft.com/en-us/projects/homeos/), [paper](https://www.usenix.org/system/files/conference/nsdi12/nsdi12-final149.pdf),  [Video](https://www.usenix.org/conference/nsdi12/towards-commodity-smarthomes-homeos), [Slides](https://www.usenix.org/sites/default/files/conference/protected-files/homeos-nsdi-talk-given-clean.pdf)

### 1. Problem

High overhead of managing and extending network devices for **smart home**: 1) growing number of devices 2) heterogeneity 3) hardware/software incompatible. 

### 2. Challenges

- Appliance abstraction: a closed, monolithic system. (manageability)
- Decentralized network-of-devices: bad portability. (extensibility: both software and hardware)

### 3. Solution

HomeOS: a PC-like abstraction for network devices

#### 3.1 Overview

<table>
    <tr>
        <td>Application layer</td><td>Tasks</td>
    </tr>
    <tr>
        <td>Management layer</td><td>Control</td>
    </tr>
    <tr>
        <td>Device functionality layer (DFL)</td><td>Device</td>
    </tr>
    <tr>
        <td>Device connectivity layer (DCL)</td><td>Topological</td>
    </tr>
    <tr>
        <td>PCs, XBox, Smartphones, TVs, ...</td><td>Heterogeneity source handled</td>
    </tr>
</table>

#### 3.2 Application Layer

Environment for develop-written codes. An application should have a manifest {rules} to specify what devices it needs. 

#### 3.3 Management Layer

1. Application manager with access control
	- Time-based access control. 
	- Applications as security principals
	- Settings should be querable
	- Sensitive devices need extra attention

2. Mediate conflicting accesses
	- Datalog access control rules: (r, g, m, Ts, Te, d, pri, a): Resource r can be accessed by users in group g, using module m, in the time window from Ts to Te, on day of the week d, with priority pri and access mode a.
	- Simplicity. User account works within a given time. Groups are in a tree hierarchy.

#### 3.4 Device Functionality Layer

Provide APIs for higher layers by using handles. 

Service interfaces = roles{operations()} ("lightswitch" role = turnOn()+turnOff())

- A new device can either use an existing role or register new roles
- OS is agnostic to the services


#### 3.5 Device Connectivity Layer

Provide handles for higher layers. 

- No understanding of device semantics
- A uniform interaction with different kinds of devices

#### 3.6 Implementation and Evaluation

C#

Developer: "music follows the lights"/"custom lights per user". 8/10 of them finished in 2h 

User: 77% completion rate

### 4. Conclusion

Bad abstractions in "smart home" result in high overhead of managing and extending network devices, which are in increasing number and mostly not compatible with each other. Traditionally, appliance abstraction provides a huge system with no potential for customization and extension. Meanwhile, decentralized network-of-devices provide little portability. So the authors present a new abstraction -- HomeOS, a PC-like abstraction for network devices. The new abstraction architecture consists of four layers. Lower layers interact with heterogenous devices and protocols. Upper layers simplify development and use of applications. HomeOS is implemented with C# and its experience shows a satisfying manageability and extensibility.

