---
id: 264
title: WPF and MVVM tutorial 01, Introduction.
date: 2009-06-03T17:41:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=264
permalink: /archive/2009/06/03/wpf-and-mvvm-tutorial-01-introduction.aspx
categories:
  - 'C#'
  - Design Pattern
  - MVVM
  - Silverlight
  - WPF
---
With Microsoft WPF technology, a new pattern is born and is going to be called <a href="http://msdn.microsoft.com/en-us/magazine/dd419663.aspx" target="_blank">MVVM</a> (Model View ViewModel). This pattern is an hybrid from the old MVC and the old MVP patterns. Why a new pattern for the presentation?

  1. First of all WPF technology is giving us a kind of technology that can completely change the approach to design and code the UI. With the VMMV we can completely design an agnostic UI that doesnâ€™t know the Model we are going to pass to it. 
  2. Recycle, I will show you in this tutorial how to simply convert a WPF application into a Silverlight app. 
  3. Better delegation and better design for a real n-tier application. In this example we will use LinqToSQL and WPF to build a complete n-tier application with the VMMV. 
  4. Something that I do not like, TESTING THE UI!! Yes we can test the UI with WPF and VMMV combination. 
  5. Abstractation. Now the view can be really abstract and you can use just a generic.xaml file and then give a style template to your model. 

This is the schema (in my opinion) on how it should work an application with WPF and the VMMV implementation:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandVMMVtutorial01TheDataModel_F169/image.png" rel="lightbox"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandVMMVtutorial01TheDataModel_F169/image_thumb.png" width="221" height="260" /></a>

  * **MODEL**: anyone that has already worked on an n-tier application knows what it is. The model is the group of entities that will be exposed. 
  * **VIEW**: the view is the graphical code XAML used to generate the UI, nothing more then that. 
  * **VIEWMODEL**: the model for the view â€¦ or â€¦ the view for the model?! Anyway is a model of the view. 

Before starting this tutorial I suggest you to download:

  * <a href="http://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E" target="_blank">Visual Studio 2008 SP1</a> and <a href="http://www.microsoft.com/downloads/details.aspx?FamilyID=ab99342f-5d1a-413d-8319-81da479ab0d7" target="_blank">NET Framework 3.5 SP1</a> 
  * The complete <a href="http://msftdbprodsamples.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=18407" target="_blank">AdventureWork</a> database template for SQL 
  * <a href="http://www.microsoft.com/express/sql/default.aspx" target="_blank">SQL Express 2008</a> 

The next tutorial will show you how to create a simple DAL layer with LinQtoSQL and how we can implement the <a href="http://www.martinfowler.com/eaaCatalog/unitOfWork.html" target="_blank">Unit of Work</a> pattern to build a simple but solid data layer for our application.

You can also download the <a href="http://wpf.codeplex.com/Wiki/View.aspx?title=WPF%20Model-View-ViewModel%20Toolkit" target="_blank">Visual Studio template here</a>.

Tags: <a href="http://technorati.com/tag/MVVM" rel="tag">MVVM</a> <a href="http://technorati.com/tag/Model View ViewModel" rel="tag">Model View ViewModel</a> <a href="http://technorati.com/tag/WPF" rel="tag">WPF</a>