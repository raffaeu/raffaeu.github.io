---
id: 267
title: LinQ to SQL, help to write better code.
date: 2009-05-24T19:10:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=267
permalink: /archive/2009/05/24/linq-to-sql-help-to-write-better-code.aspx
categories:
  - 'C#'
  - Design Pattern
  - NET World
---
In this days I am working hard with LINQ to SQL, that in my opinion is still better then the EF beta 4, for just a couple of fundamental reasons:

  1. Better support to the pattern Active Record.   
    Instead of Entity Framework LINQ allows me to draw any kind of relationship from one entity to another one. I can specify which primary key (entity key) i want to use and the designer doesnâ€™t force me to VALIDATE my domain to perfectly reflect the database.
  2. Better support to LINQ sintax.   
    I show you 1 very simple example. SELECT WHERE IN xxx.   
    This can be simply accomplished with LINQ to SQL with the syntax array.contains(x.entity_field) and works perfectly. With EF this is still impossible.
  3. Finally better support over the web.   
    It is still very hard to find good example on how to write nice queries with EF.   
    With LINQ to SQL you can download this awesome software: <a href="http://www.linqpad.net/" target="_blank">LINQPad</a>.

A short screenshot:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/LinQtoSQLhelptowritebettercode_10DB6/image.png" rel="lightbox"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/LinQtoSQLhelptowritebettercode_10DB6/image_thumb.png" width="260" height="205" /></a> 

So, I will still work with LINQ to SQL for the moment. Of course I am not using anymore NHibernate now!!