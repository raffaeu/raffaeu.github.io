---
id: 74
title: NHibernate cache system. Part 3
date: 2011-12-29T20:24:43+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=74
permalink: /archive/2011/12/29/nhibernate-cache-system-part-3.aspx
categories:
  - 'C#'
  - NHibernate
---
In this <a href="http://blog.raffaeu.com/archive/2011/12/31/nhibernate-cache-system-part-1.aspx" target="_blank">series</a> of <a href="http://blog.raffaeu.com/archive/2011/12/31/nhibernate-cache-system-part-2.aspx" target="_blank">articles</a> we saw how the cache system is implemented in NHibernate and what can we do in order to use it. We also saw that we can choose a cache provider but we didn’t have a look yet at what providers we can use.

I personally have my own opinion about the 2nd level cache providers available for NHibernate 3.2 and I am more than happy if you would like to share with me your experience about it.

Below is a list of **the major and the most famous** 2nd level cache providers I know:

[<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/393cba97-be0a-42ac-8e52-74b4e762a248image_thumb.png" width="876" height="257" />](http://raffaeu.com/wp-content/uploads/2013/03/c5eb0517-92b7-4c7f-b006-12431131291dimage_2.png)

I would personally suggest you to answer the following questions in order to understand what is the cache provider you need for your project:

  * Size of you project (small, standard, enterprise) 
  * Cache technology you have already in place 
  * Quality attributes required by your solution (scalability, security, …) 

### SysCache

<div>
  SysCache and the most recent SysCache2 is a cache provider built on top of the old ASP.NET cache system. It is available from ASP.NET cache provider (<strong>NHibernate.Cache.SysCache.dll</strong>)
</div>

It is an abstraction over ASP.NET cache so it can’t be used in a non-web application. It works but Microsoft suggest to do not use it for non-web applications.

The cache space is not configurable so it is the same for different session factories … really dangerous on a web application that requires isolation between the various users ([http://](http://ayende.com/blog/1708/nhibernate-caching-the-secong-level-cache-space-is-shared)[ayende.com/blog/1708/nhibernate-caching-the-secong-level-cache-space-is-shared](http://ayende.com/blog/1708/nhibernate-caching-the-secong-level-cache-space-is-shared))

**Useful for**: small in house projects, better for web projects hosted in IIS

### NCache

NCache is a distributed in-memory Object cache and a distributed ASP.NET Session State manager product.

It is able to synchronizes cache across multiple servers so it is designed also for the enterprise.

It provides dynamic clustering & cache configuration for 100% uptime for a real scalable architecture.

Cache reliability through data replication across servers

InProc/OutProc cache for multiple processes on same machine

API identical to ASP.NET Cache

It is available for download and trial here [http://](http://www.alachisoft.com/ncache/edition-comparison.html)[www.alachisoft.com/ncache/edition-comparison.html](http://www.alachisoft.com/ncache/edition-comparison.html), it is a third party provider and **it is not free**.

**Useful for:** medium to big applications that are designed to be scalable

### MemCache

MemCache is a famous Linux cache system designed exclusively for the enterprise. It is a complex enterprise cache system based on Linux platform that provide a cache mechanism also for NHibernate.   
It is pretty easy to be scaled on a big server farm because it is designed to do so   
It does not require licensing cost because it’s an OSS and it is a well known system with a big community.

The following picture represents the core logic of MemCache:

[<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/6cf9e1d4-2ae6-4f92-a36f-002bb590e58dimage_thumb_1.png" width="515" height="251" />](http://raffaeu.com/wp-content/uploads/2013/03/d13fc473-b318-45e9-bd04-10126f2dc26aimage_4.png)

The only downside is that it requires a medium knowledge of Linux OS in order to be able to install and configured it.

**Useful for:** enterprise applications that are designed to be scalable

### Velocity, a.k.a. AppFabric

Velocity has been now integrated in AppFabric and it is the cache system implemented by Microsoft for the enterprise. It requires AppFabric and IIS and it can be used locally or with Azure (does it make sense to cache in the cloud?? <img style="border-bottom-style: none; border-left-style: none; border-top-style: none; border-right-style: none" class="wlEmoticon wlEmoticon-flirtmale" alt="Flirt male" src="http://raffaeu.com/wp-content/uploads/2013/03/33acf3f0-fcc1-4c4c-98a8-869dfde42629wlEmoticon-flirtmale_2.png" />).

  * AppFabric Caching, provides local caching, bulk updates, callbacks for updates, etc&#8230; so this is why it&#8217;s exciting over something like MemCache which doesn&#8217;t provide these features Out of the Box. 
  * For enterprise architectures, really scalable, Microsoft product (there may be a license requirement) 

**Useful for:** enterprise applications that are designed to be scalable