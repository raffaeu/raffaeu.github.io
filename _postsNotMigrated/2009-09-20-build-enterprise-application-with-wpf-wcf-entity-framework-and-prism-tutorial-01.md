---
id: 243
title: Build enterprise application with WPF, WCF, Entity Framework and Prism. Tutorial 01.
date: 2009-09-20T16:29:39+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=243
permalink: /archive/2009/09/20/build-enterprise-application-with-wpf-wcf-entity-framework-and-prism-tutorial-01.aspx
categories:
  - 'C#'
  - Design Pattern
  - Silverlight
  - WCF
  - WPF
---
<blockquote style="border-bottom: black 1px solid; border-left: black 1px solid; padding-bottom: 5px; background-color: #ffff00; margin: 5px; padding-left: 5px; padding-right: 5px; border-top: black 1px solid; border-right: black 1px solid; padding-top: 5px">
  <p>
    <strong style="color: #ff0000">Update: </strong>source code and documentation are now available on <a href="http://www.codeplex.com" target="_blank">CodePlex</a> at this address: <a title="http://prismtutorial.codeplex.com/" href="http://prismtutorial.codeplex.com/">http://prismtutorial.codeplex.com/</a>
  </p>
</blockquote>

In these series of articles I will show you how to build an enterprise application using the most recent Microsoftâ€™s technologies like: WPF with PRISM for the UI, Entity Framework for the Data Layer and WCF for the persistence service.

### Specifications.

We will build a 3 tiers application that will spread the responsibility through 3 different machines (virtually because the Visual Studio solution is only one).

  1. SQL Server   
    It will contain the database and the database objects 
  2. IIS 7   
    Data access layer with Entity Framework   
    WCF host service for the persistence   
    WCF host service for the security access 
  3. PRISM (Composite WPF application)   
    This layer will use the PRISM pattern to render a WPF modular application 

This is the schema of our tiers: 

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_E2A0/image.png" rel="lightbox[TUTORIAL]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_E2A0/image_thumb.png" width="260" height="189" /></a>

### Prerequisites and Source code.

As I did for my previous series of post about <a href="http://blog.raffaeu.com/archive/2009/06/03/wpf-and-vmmv-tutorial-01-introduction.aspx" target="_blank">MVVM and WPF</a>, I will share a CodePlex project for anyone of you that want to reuse the project for professional or testing purposes, I will use the Adventure works database available in the CodePlex web site and I will use the PRISM pattern.

Prerequisites:

  1. <a href="http://msdn.microsoft.com/en-us/vstudio/default.aspx" target="_blank">Visual Studio 2008</a> with SP1 available for <a href="http://www.microsoft.com/downloads/details.aspx?FamilyID=83c3a1ec-ed72-4a79-8961-25635db0192b&displaylang=en" target="_blank">trial on Microsoft web site</a>. 
  2. <a href="http://www.microsoft.com/downloads/details.aspx?FamilyID=ab99342f-5d1a-413d-8319-81da479ab0d7&displaylang=en" target="_blank">NET 3.5 SP1</a> available on MSDN. 
  3. <a href="http://www.microsoft.com/express/sql/download/" target="_blank">SQL Server 2008</a> Express Edition. 

As we are going to use some very well known patterns and methodologies, like: Factory pattern, Inversion of Control, POCO objects, Test Driven Development; I suggest to anyone of you to have a look at:

  1. Martin Fowler <a href="http://martinfowler.com/articles.html" target="_blank">articles and patterns</a>. 
  2. Gang of Four <a href="http://www.dofactory.com/Patterns/Patterns.aspx" target="_blank">NET patterns</a>. 
  3. Composite UI Application for <a href="http://www.codeplex.com/CompositeWPF" target="_blank">Silverlight and WPF</a>. 
  4. At least, basic knowledge of <a href="http://msdn.microsoft.com/en-us/library/ms752059.aspx" target="_blank">XAML syntax</a> and how <a href="http://windowsclient.net/wpf/" target="_blank">WPF works</a>. 

Those are part of the patterns and methodologies we will use. It is not mandatory to know them but if you have at least a look, it will be easier to follow the next articles.

For today itâ€™s enough, be ready with your PC and from the next article we will start to build the database and the Data access layer.

Tags: <a href="http://technorati.com/tag/WPF PRISM" rel="tag">WPF PRISM</a> <a href="http://technorati.com/tag/Entity Framework" rel="tag">Entity Framework</a> <a href="http://technorati.com/tag/WCF" rel="tag">WCF</a>