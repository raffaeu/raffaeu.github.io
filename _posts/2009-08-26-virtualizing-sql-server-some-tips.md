---
id: 245
title: Virtualizing SQL Server, some tips.
date: 2009-08-26T14:17:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=245
permalink: /archive/2009/08/26/virtualizing-sql-server-some-tips.aspx
categories:
  - NET World
  - SQL Server
  - Technical Documents
---
I just got these considerations about virtualizing SQL Server on VMWare from my Boss and I would like to share them with you.

**<u>General comments on problems encountered and why people not virtualizing SQL</u>**

  1. Storage ( most people just do not have enough spindles to start with and then when people consolidate they use less spindles not more, error in design and understanding of basic i/o principles )
  2. Queue lengths are a problem so add as many paths to storage as possible.   
    Using vmware makes sequential reads look more like random reads due to interleaving of traffic across vmâ€™s
  3. Ensure all dbs ( data, temp, log etc ) are on separate vmdks or preferably rdms, improves i/o queue throughput
  4. Memory ( ensure you set min, max reservations per vm to avoid swapping )
  5. 1 database instance per vmhost ( to allowing you to tweak memory / priorities as required )
  6. Use the latest intel chips, as provide biggest reduction in latencies for vm tasks

Following a thru 6 results in linear performance until the cpu saturates around 90%. 

> _Basically they said VMware is never the bottleneck &#8230;_

Assuming the rules above are followed you should then consider tweaking the following:

  1. Tweak the o/s first to allow large pages and ensure alignment with storage ( gives +6% improvement )
  2. Turn on sql priority boost in SQL and ensure correct O/S drivers installed in host ( **gives +14% when used with 1.** )
  3. Turn on static transmit coalescing in ESX networking ( gives +1%, so they advise donâ€™t bother )

They are interesting considerations. Honestly, I know there are some â€œhow toâ€ and some â€œbest practicesâ€ when you prepare a SQL environment. Using separate HD for separate files (mdf, ldf), keep system database separate from user database in different disks, split big tables in different location, maintain the indexes and so on â€¦ Unfortunately when you reflect this to a Virtual Environment, everything changes.

I didnâ€™t find yet a good documentation about virtualizing SQL but I just tried to build a Clustered environment with Windows 2008 R2 x64 and SQL 08 x64 SP1 and the Quad Core I used for my experiments was not enough â€¦

<img src="http://raffaeu.com/wp-content/uploads/2013/03/62c892eb-f0e2-40a4-b9f3-506a46f255e0smiley-smile.gif" border="0" alt="Smile" />

Tags: <a href="http://technorati.com/tag/SQL server" rel="tag">SQL server</a> <a href="http://technorati.com/tag/vmware" rel="tag">vmware</a> <a href="http://technorati.com/tag/virtualize sql server" rel="tag">virtualize sql server</a>