---
layout: post
title: "[Repost] Numbers everyone should know"
date: 2013-01-30 11:19
comments: true
categories: 
---

I seldom repost unoriginal notes in this blog. However, this time... 

Jeff Dean, a brilliant engineer at Google, gave a talk a while ago listing the numbers every engineer should know. I copied these from here.

--------------

<table>
 <tbody><tr><td>  L1 cache reference                   </td><td class="c"> 0.5 ns                               
 </td></tr><tr><td>  Branch mispredict                    </td><td class="c"> 5 ns                                 
 </td></tr><tr><td>  L2 cache reference                   </td><td class="c"> 7 ns                                 
 </td></tr><tr><td>  Mutex lock/unlock                    </td><td class="c"> 100 ns                               
 </td></tr><tr><td>  Main memory reference                </td><td class="c"> 100 ns                               
 </td></tr><tr><td>  Compress 1K bytes with Zippy         </td><td class="c"> 10,000 ns       </td><td class="c"> 0.01 ms 
 </td></tr><tr><td>  Send 1K bytes over 1 Gbps network    </td><td class="c"> 10,000 ns       </td><td class="c"> 0.01 ms 
 </td></tr><tr><td>  Read 1 MB sequentially from memory   </td><td class="c"> 250,000 ns      </td><td class="c"> 0.25 ms 
 </td></tr><tr><td>  Round trip within same datacenter    </td><td class="c"> 500,000 ns      </td><td class="c"> 0.5 ms  
 </td></tr><tr><td>  Disk seek                            </td><td class="c"> 10,000,000 ns   </td><td class="c"> 10 ms   
 </td></tr><tr><td>  Read 1 MB sequentially from network  </td><td class="c"> 10,000,000 ns    </td><td class="c"> 10 ms   
 </td></tr><tr><td>  Read 1 MB sequentially from disk     </td><td class="c"> 30,000,000 ns   </td><td class="c"> 30 ms   
 </td></tr><tr><td>  Send packet CA-&gt;Netherlands-&gt;CA      </td><td class="c"> 150,000,000 ns  </td><td class="c"> 150 ms  
 </td></tr></tbody>
</table>

--------------

Where

- 1 ns = 10^(-9) seconds
- 1 ms = 10^(-3) seconds