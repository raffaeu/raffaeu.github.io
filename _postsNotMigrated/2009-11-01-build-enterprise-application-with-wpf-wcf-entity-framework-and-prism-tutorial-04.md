---
id: 238
title: Build enterprise application with WPF, WCF, Entity Framework and Prism. Tutorial 04.
date: 2009-11-01T15:41:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=238
permalink: /archive/2009/11/01/build-enterprise-application-with-wpf-wcf-entity-framework-and-prism-tutorial-04.aspx
categories:
  - 'C#'
  - Design Pattern
  - WCF
  - WPF
---
## Service Oriented Application.

### Introduction.

The first problems I always encounter when I design a scalable, multi-tier application are:

  * Work on a disconnected environment but be able to work with the data previously downloaded.
  * Keep always the data up to date, notify the users when the data changes.
  * Threading and UI performances, do not provide frozen screen when the user requests the data.
  * Synchronization from the database and the disconnected environment.

Those are common problems of a complex software and can be solved using a SOA service solution and WPF for the UI. Of course WPF and WCF by themselves cannot help us without a strong and well designed solution.

The idea of having a SOA service for the communication will give us the ability to design something like this:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_DC9B/image.png" rel="lightbox[tutorial]"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_DC9B/image_thumb.png" width="260" height="233" /></a>Â  

Where we will have 1 or more databases, remotely stored somewhere; a distributed SOA service that will allow our software to operate with the remote data, and maybe to have also some security concerns; the final Client application that will be build using some techniques, in order to use the full power of this kind of solution.

Of course this sketch is fancy and cool but it doesnâ€™t give us anything more than a .PNG file that we can present to the CTO or to the customer! What you have to do in order to transform the sketch in a real solution, itâ€™s complex and hard.

### Consideration when working with SOA Services.

The first consideration we have to do, if we decide to leave the DAL on a remote web service, and work with a completely disconnected environment, is the **<u>data synchronization</u>** and the **<u>data</u>**Â **<u>concurrency</u>**.

Letâ€™s make a very simple self explanatory workflow.

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_DC9B/image_3.png" rel="lightbox[tutorial]"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_DC9B/image_thumb_3.png" width="260" height="111" /></a>Â  

We have the order **10-AED-2020** and is opened remotely by 3 employees in the same time. Everyone makes a change to the row, and then they all try to save the changes. What will happen? â€¦ a mess â€¦ <img src="http://raffaeu.com/wp-content/uploads/2013/03/d3aaf29c-47fb-446f-b2f6-7b62cdb08554smiley-wink.gif" border="0" alt="Wink" />

Usually, what I do, is to â€œ_virtually_â€ lock the record so when someone else try to open the row, the UI notifies that the row is already opened by someone else. Then when the â€œ_someone elseâ€_ save or close the row, we notify the changes to everybody.

The second problem can be called **<u>routing</u>** **<u>messaging</u>** **<u>approach</u>**.   
Also for this problem I have drawn a simple workflow. (Sorry guys but I love Visio!) <img src="http://raffaeu.com/wp-content/uploads/2013/03/6c52c5d3-1132-4f47-8dc4-b529de55df34smiley-tongue-out.gif" border="0" alt="tongue-out" />

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_DC9B/image_4.png" rel="lightbox[tutorial]"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_DC9B/image_thumb_4.png" width="260" height="218" /></a> 

The process is described below:

  1. The Vendor requests for a **new order**
  2. The **software** submit the request to the **service** and **<u>wait</u>**
  3. The **service** creates the order
  4. The **service** gives back to the **client** the new order created

As you can see we can have two different type of message: _a single way message (the request)_ and a _request and response_ message _(create order)_, that has a **delay time**.

Of course there are many others considerations but for now this is enough for what we are going to do.

### Some interesting articles.

Before writing XML or C# code I wish you will have the time to read those articles that I have found really interesting, especially if you had never step into an n-tier application.

<a href="http://msdn.microsoft.com/en-us/magazine/dd569749.aspx#id0430125" target="_blank">MSDN â€“ Build service oriented app</a>

<a href="http://msdn.microsoft.com/en-us/magazine/cc700340.aspx" target="_blank">The Entity Framework In Layered Architectures</a>

<a href="http://www.designpatternsfor.net/Default.aspx?pid=99" target="_blank">Patterns for Flexible WCF services</a>

In the next tutorial we will write the code to exposes our entities through WCF, but of course we will not expose directly them but an â€œ_aliasâ€_. I will show the differences from using a DTO or something else.

Stay tuned!

Tags: <a href="http://technorati.com/tag/WCF" rel="tag">WCF</a> <a href="http://technorati.com/tag/WPF" rel="tag">WPF</a> <a href="http://technorati.com/tag/Prism" rel="tag">Prism</a> <a href="http://technorati.com/tag/Composite application" rel="tag">Composite application</a>