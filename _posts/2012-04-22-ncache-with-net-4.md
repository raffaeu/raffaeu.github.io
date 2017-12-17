---
id: 49
title: NCache with .NET 4
date: 2012-04-22T20:17:18+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=49
permalink: /archive/2012/04/22/ncache-with-net-4.aspx
categories:
  - NCache
  - NET World
---
I am very excited about this new series I am publishing today. It’s all about caching, a very useful portion of your architecture that should be seriously taken into consideration, especially if you are designing a web application.

Actually, I used <a href="http://www.alachisoft.com/ncache/" target="_blank">NCache</a> with <a href="http://nhforge.org/Default.aspx" target="_blank">NHibernate</a> and I can only say that it is a very good and valuable product. Of course most of you may believe that I got a free license so this is the reason of my review. Actually I am reviewing this product because I found it pretty good and that’s why the review came up with a short series of articles.

I will show you the following features of <a href="http://www.alachisoft.com/ncache/" target="_blank">NCache</a>:

  * <a href="http://blog.raffaeu.com/archive/2012/04/22/ncache-with-net-4.aspx" target="_blank">NCache architecture</a> 
  * <a href="http://blog.raffaeu.com/archive/2012/04/22/ncache-simple-caching-of-objects.aspx" target="_blank">Simple caching of objects</a> 
  * Caching of complex graph 
  * Control the lifetime of a cache 

### Introduction

From <a href="http://www.alachisoft.com/ncache/" target="_blank">NCache</a> documentation:

> <a href="http://www.alachisoft.com/ncache/" target="_blank">NCache</a> is a clustered caching solution that makes sharing and managing data in a cluster as simple as on a single server. It accomplishes this by coordinating updates to the data using cluster-wide concurrency control and replicating and distributing data modifications across the cluster using the highest performing clustered protocol available. The primary purpose of <a href="http://www.alachisoft.com/ncache/" target="_blank">NCache</a> is to help improve performance of .NET applications that would otherwise make expensive trips to database systems, web services, mainframes, or other systems across the network.

Below is a very simplified diagram of NCache architecture (_please forgive me for the simplicity of this architecture design)_:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/23942c85-2a87-47f5-a291-103ccfacabc7image_thumb.png" width="260" height="242" />](http://raffaeu.com/wp-content/uploads/2013/03/017cda4f-4212-49e3-ac7c-4784a6eaef33image_2.png)

### So, how does it work?

The explanation is pretty simple but in the same time is represented by a complex architecture. You can have two different type of Cache, a local cache or a remote cache. The first one will work because the client has a cache server installed locally, the second one will work with the client accessing the remote cache server from the network.

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/960fc9e4-e392-4b38-8d97-546ff53cfb7bimage_thumb_1.png" width="260" height="181" />](http://raffaeu.com/wp-content/uploads/2013/03/8a4fab1c-2f88-4944-8a80-05736b1c8efeimage_4.png)

Of course you can easily swap from a local configuration to a remote configuration without even need to restart or rebuild your application, plus you can easily add or remove cache clusters to your architecture using an “hot swap” technology that doesn’t require any reboot or re-configuration.

### Installation

Let’s try to get practical and let’s write some code, otherwise this blog post will get really boring! <img class="wlEmoticon wlEmoticon-winkingsmile" style="border-top-style: none; border-bottom-style: none; border-right-style: none; border-left-style: none" alt="Winking smile" src="http://raffaeu.com/wp-content/uploads/2013/03/45a2716c-a9ba-407d-9403-79a3f01b8a8dwlEmoticon-winkingsmile_2.png" />

<p align="left">
  In order to use <a href="http://www.alachisoft.com/ncache/" target="_blank">NCache</a> you have to go to this web address and download a 60 days trial version: <a title="http://www.alachisoft.com/download.html" href="http://www.alachisoft.com/download.html">http://www.alachisoft.com/download.html</a>. There are three major versions: NCache for .NET (x86 and x64) in two flavors, Enterprise and Professional, NCache for Java (x86 and x64) and NCache express (a free version with a limited amount of features). In this demo I will use the full version for .NET in x64 bit Professional. The setup for the professional edition is about ~30 Mb and it’s pretty quick to install.
</p>

<p align="left">
  <strong>Note:</strong> the only problem you may encounter is with Windows UAC because <a href="http://www.alachisoft.com/ncache/" target="_blank">NCache</a> works better if UAC is disabled. To know more about UAC go here: <a href="http://msdn.microsoft.com/en-us/library/ms229742.aspx" target="_blank">MSDN Windows ACL</a>
</p>

<p align="left">
  The Wizard will propose you three different types of installation, like the screenshot below:
</p>

<p align="left">
  <a href="http://raffaeu.com/wp-content/uploads/2013/03/443ec435-0afc-4c9f-b79c-bd16ff5ad5daSNAGHTMLe4f4ea.png"><img title="SNAGHTMLe4f4ea" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="SNAGHTMLe4f4ea" src="http://raffaeu.com/wp-content/uploads/2013/03/dba4d0d5-8710-4aec-bc7a-52c1a7eb5b5aSNAGHTMLe4f4ea_thumb.png" width="292" height="230" /></a>
</p>

<p align="left">
  In my case I need a developer license because I will run everything from my machine and I need also to have access to NCache API. The cache server installation is used to install a new cluster while the Remote Client installation is used to install the client connection components used by your applications to access the remote cache servers.
</p>

As soon as NCache is installed you will get a new icon in your start menu (_in my case is a tile because I have Windows 8_): 

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/6dc20f0e-c9a7-4f9b-b560-c53ac337b24aimage_thumb_2.png" width="131" height="132" />](http://raffaeu.com/wp-content/uploads/2013/03/53132a56-fc62-4364-8cc2-9d4e5cbb275fimage_6.png)

In the next posts I will show you how to configure your first cache and how to write a WPF application that will work against this cache.

Stay tuned