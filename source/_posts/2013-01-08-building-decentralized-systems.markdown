---
layout: post
title: "Building Decentralized Systems"
date: 2013-01-08 07:43
comments: true
categories: System
published: false
---


LEC 2: UseNet and Gossip Messaging: UseNet Message [RFC 1036](http://www.ietf.org/rfc/rfc1036.txt)
---------------------------------
1. Every node is not connected directly to each other.

1. gossip protocol (= epidemic protocol)

1. **Message-ID**: unique, avoid infinite loop forwarding

1. Every node picks up one random neighbor and forward the message: have you ever seen this message? And the next.

1. TCP time's up in 20s, but how about a node is down for 2 hours?

- Anti entropy (classic): Nodes periodically wake up and compare Message-ID lists. *not very effective*: have to remember IDs. doesn't care who send the message, only Message-ID (vulnerable). 

[lab](): TODO make sequence explicit. 

- [rumor-mongering](http://en.wikipedia.org/wiki/Gossip_protocol)

easy to spread, hard to eliminate
How? -> "Kill message": rumor a new message to a delete certain message, a polite optional request

censorship resistant, so much noise



Lab 1: Gossip Messaging
------------------------------

### User Datagram Protocol

Simple, stateless, no handshaking, unreliable

Checksums

Port: numbers for addressing different functions

avoid overhead at network interface level (error checking, correction)

Suitable for realtime system, lack of retransmission delays
for unidirectional

For error correction, use TCP, SCTP

#### Service Ports

port number 16 bit int (0 ~ 65535) -> **multiplexing**
1. 0 ~ 1023: common, well-known services
2. 1024 ~ 49151: IANA-registered services
3. 49152 ~ 65535: any purpose: *ephemeral ports*

#### Packet Structure
from [wikipedia](http://en.wikipedia.org/wiki/user_datagram_protocol)

Header contains 4 fields (4*16 bit).
- IPv4: source port number and checksum are optional 
- IPv6: source port number is optional

1. Source Port Number
- Zero if not used
- Client: ephemeral
- Server: well-known
2. Destination Port Number
- Client: ephemeral
- Server: well-known
3. Length
- Length of the entire datagram = header + data
- 8 bytes <= length <= 65535 bytes (8 bytes + 65527 bytes)
- IPv4 data length = 65507 bytes (65535 - 8 UDP header - 20 IP header)
4. Checksum
- All zero if not used
- All 16-bit words are summed using one's complement
- IPv4: *pseudo header* 
- IPv6: mandatory and *pseudo header*


Lab 1 Notes
-----------------------------------
message = serialized instance of QVariantMap = key/value dict
keys=QSTring, values=QVariant(wrapper)
QDataStream serialize QVariantMap to QByteArray
send it via QUdpSocket::writeDatagram() to each of the ports 
receive: connect readyRead() signal in QUdpSocket to a new slot; QDataStream deserialize QByteArray to QVariant

### Part 3 A Simple Gossip Protocol
2 Problems
- unreliability results in loss, duplication
- *asymetric: indirect communication*
 
Got copies of whatever messages

#### Gossip in Peerster

2 things to do
- Acknowledge messages they have received. If sender does not receive an acknowledgment, resend.
- no loop
**Unique ID**

pair of values: origin, sequence number (1:),
vector clock

1. Rumor Message
QVariantMap containting 3 keys:
QVariantMap<QString, QString>, QVariantMap<QString, QString>, QVariantMap<QString, quint32>
<"ChatText", "Hi">, <"Origin", "tiger.zoo.cs.yale.edu">, <"SeqNo", 23>

2. Status Message
QVariantMap <QString "want", <"tiger.zoo.cs.yale.edu", 4>> 
表示看过4以前的，没看过4

#### Rumormongering

receive and pick up a random neighbor
wait for a response as *status message* 
  if receive acknoledgment, compare the status message, if there are messages remote peer has not seen, then send the messages
  if remote peer has new messages, send my status message (remote peers send the messages *one at a time*)
  if sync, the starter *qrand()%2*, *either* to pick up a new random neighbor, *or* cease.
or timeout

keep order from (1:)

Temp: 4 ports as a line topology, (1)-(2)-(3)-(4)

ID: QHostInfo::localHostName() + qrandom()

QTimer to have a handler reinvoked after a time span no receive (1 or 2 s)


#### Anti-entropy
QTimer, 10s to send a status messagge to randomly chosen neighbor



LEC3: Location, Identity, and Routing
--------------------------

### Postel's (Robustness) Principle 严于律己，宽以待人
- Be liberal in what you accept
- Be conservative in what you produce

How to create a one2one reliable connection?
Chanllenges: 

#### routing(identity vs. location)

Location: IP addr., hostname.domain name,
(-) portability

##### Source Routing
Path: a|b|C|d|e|user
(-) loop, inefficiency, broken,

##### Routing Table
identifier

1. Master Router Node

2. Every node keeps the map (K-Value) 

(-)Scale, malicious, loop
Latency detect, good/bad namelist, 


Firewalls, NATs, and Getting Through Them
---------------------------------------

Crypto
---------------------------------------

Black box

### Concepts

1. Hash

Crypto Hash:

- hard to find collision
- One way

(+)

- efficient communication
- verification: hashes for files

2. Symmetric cipher

ceasar cipher: + 13

AES, CBC, CTR

3. PKE (PUBLIC KEY ENCRYPTION)

[Diffie-Hellman Key Exchange](http://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)

    K.public -- k.private
      \/              \/
    "foo" -> PKE -> "xxz"
    
    +---+---+           +---+---+
    |foo|sig| <- PKE -< |foo|sig|
    +---+---+           +---+---+
       /\                  /\

Alice: A = g^a, give it to B

Bob: B = g^b, give it to A

then, they got mutual g^(a*b)

basis: hard decomposition of large prime numbers

4. Signatures

Alice:

R = g^r ---------- (R, "foo") --------------\>

Decentralized Identities
===========================================

Man-in-the-middle attack

How to solve the problem?


[PKI](http://en.wikipedia.org/wiki/Public_key_infrastructure)
 
A public-key infrastructure (PKI) is a set of hardware, software, people, policies, and procedures needed to create, manage, distribute, use, store, and revoke digital certificates.

1. Traditional CAs

[Certificate authority](http://en.wikipedia.org/wiki/Certificate_authority)

Web Browser: Root CertsA -> subCA -> subCA ->... 

PGP = Pretty Good Privacy

2. "Web of Trust" 

Alice (B says A="A") ---- Bob (A says B="B" | C says B="B") ---- Charlie (B says C="C")

Vulnerable: 

3. Self-Certifying ID

URL: http://doodle.com/jsadl332pt24p

pub key + "I am Bryan" -> hash as Name



Unstructured Search
===============================

Approaches

1. Hooding

2. Index nodes / Super peers

3. Expanding Horizon

(-) Tragedy of commons

4. Freenet

5. Bubble Storm

Birthday paradox


================================

NFS (Sun Network FS), AFS (Andrew FS), Coda (disconnected operation), LOCUS, Bayou

NFS
--------------

Cilent: mount foo:vol/vol  \|  fd \*open("filename"); read(fd, buffer); write(...)

file discriptor 

open(O_create| 0\_EXA) to create "mydata.txt" "mydata.txt.lock"


three way diff


Content Distribution
=

### 1. Akamai

NY Times has a lot of webpages. Akamai makes deals with ISPs to cache the pages. Akamai redirect the quest to the local ISP.

(+) faster, good for ISP/Server/Client

### 2. CoralCDN

More open than akamai

bford.info.nyud.net

1. DNS reg -> Coral DNS server
2. Ping the user, estimate the location
3. select coral proxy, respond to DNS
4. HTTP reg -> proxy

Coral proxy DSHT (D Sloppy HT)

slop: If no cache is found and the last one proxy in the DHT is full, then the new cache would be stored into the near ones. It violates DHS's principles.

(-) latency, security, 

### 3. BitTorrent


instruction count


e.g. PlanetLab


Recommendation System
===================

### 1. Credendce

Facotrs

- popularity
- lifetime
- other user's behavior
- other user's rating

Issuses

- misleading info
- absolute vs. relative ratings
- Anchoring effect
- Sybil attacks -> ballot shuffing

Figre

    ------+-------+-------+-------+-------+ 
    Files | UserA | UserB | UserC | UserD |
    ------+-------+-------+-------+-------+ 
    F1    |  -1   |  ?    |  +1   |  +1   |
    F2    |  +1   |       |  +1   |       |
    F3    |       |  +1   |  -1   |  -1   |
    F4    |  +1   |  +1   |  -1   |  +1   |
    F5    |  -1   |       |       |       |
    ------+-------+-------+-------+-------+ 
    wight |  w1   |  w2   |  w3   |  w4   |
    ------+-------+-------+-------+-------+ 

compute weights according to similarity of me and corresponding user.

         0.5
	 (A)-------(B)
	  | \       |
	.2|   \ .7  | 1
	  |     \   |
	  |       \ |
	 (D)-------(C)
	      .5


             

### 2. SumUP

Two regions of a graph, honest and Sybil. A in honest starts a vote. How to go on?

A generates 10 tickets. Spread tickets in the graph randomly. 

Graph with N nodes, then:
- 2 tickets -> ~2 nodes
- 4 tickets -> ~4 nodes
- ...
- N tickets -> ~N nodes
- 2N tickets -> ~NlogN nodes

### 3. dSybil

Small fraction of good nodes. 


Digital Preservation
=============

1. Media

- format dosdescence
- physical deterioration
- power spikes, floods,

physical bit-rot

2. Logical

- old file formats
- unix string 
- run old software 
- run old OS on a virtual machine or an old machine

3. "Intentional" bit-rot

- Dongles
- copy-protect disks

Anonymous communication
=======================

Anonymity vs. Accountability

1. assume it

- good? depends

2. use "public" machine

- [+] physical info cached
- [-] private info cached/cookies
- [-] malware/viruses

3. use proxy

4. multihop relays

- email: mixmaster/mixminior
- Tor: interactive web. Basic version: a path transferring onion messages. Advanced version: onion tunnels. 


Accounting
==========

Counter-Strike Game

		fireButtonPressed() {
			if (bullets_in_clip > 0) {
				bullets_in_clip--;
				fire();
			} else 			
				reload;
		}
		
	Server---+-----clientA
	         |
			 +-----clientB
			 |
			 +-----clientC
		 
Peer Review

Tamper-evident log, hash chain (hashes all previous logs)

Msg with signature

Match msg in logs

Free Last Lecture
=================

- StegoTorus
- SkypeMorph
- FreeFlow
- Curveball / Decoy routing

Fault tolerance
===============

RSM 

Primary/backup to ensure a single order of ops

Paxos

lets all nodes agree on the same value despite node failures, network failures, and delays.

