---
id: 114
title: 'TypeMock tutorial #01. Startup.'
date: 2011-06-19T18:48:47+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=114
permalink: /archive/2011/06/19/typemock-tutorial-01-startup.aspx
categories:
  - ALT
  - 'C#'
  - TDD
  - TypeMock
---
The best way to learn a tool is to try it, test it and then finally use it over your code. Of course if the tool provides also a great community support and a great documentation the task will be easier.

Some weeks ago we started to adopt a wonderful tool to create mockups and other TDD fancy stuff, the tool is <a href="http://www.typemock.com" target="_blank">TypeMock</a>.

The idea I got is to create a series of tutorials about <a href="http://www.typemock.com" target="_blank">TypeMock</a> and provide to you a piece of a code to download a full license of this tool. At the end of the series (probably 1 month) you will be able to enable your 10 days trial into a full working license. I will create a sort of bid and from all my readers that will contact me to get the license I will come up with one or two free licenses.

**Guys, consider that one license of TypeMock is 800 $ !!** 

## <font color="#2e2e2e">Setup and Installation</font>

<font color="#2e2e2e">In order to start right away with TypeMock you need to download the latest version of the tool (at this time they have the version 6.0.10) but be careful because they upload new versions often. The product you need to download for .NET is <a href="http://www.typemock.com/isolator-product-page" target="_blank">Isolator.NET</a>. They also provide additional tools that we will analyze during this series, like:</font>

  * <font color="#2e2e2e">TestDriven.NET, an integrated test runner for Visual Studio</font> 
  * <font color="#2e2e2e">Isolator for Sharepoint</font> 
  * <font color="#2e2e2e">Isolator for ASP.NET and ASP.NET MVC</font> 
  * <font color="#2e2e2e">TeamMate, a useful tool to monitor your TDD approach</font> 
  * <font color="#2e2e2e">Isolator ++, the same version but for C++</font> 
  * <font color="#2e2e2e">TestLint, a nice tool that will help you to develop your TDD skill</font> 

<font color="#2e2e2e">and more.</font>

<font color="#2e2e2e">After you have downloaded the setup (7 Mb) you will have to follow a very straightforward setup wizard with only two options available; use the advanced option and install everything including the samples for .NET.</font>

<font color="#2e2e2e">That’s it, you are now ready to go!</font>

## <font color="#2e2e2e">File Location</font>

<font color="#2e2e2e"><a href="http://www.typemock.com" target="_blank">TypeMock</a> is installed on your C:\ drive and depending on where you choose to install it, you should have a folder called TypeMock/Isolator/6.0 on your Program Files directory. Inside this folder you can find all the <strong>assemblies </strong>available from <a href="http://www.typemock.com" target="_blank">TypeMock</a>.</font>

<font color="#2e2e2e">You do not need to use them directly as <a href="http://www.typemock.com" target="_blank">TypeMock</a> is also installed on your GAC folder but if you plan to work with C.I. (<a href="http://martinfowler.com/articles/continuousIntegration.html" target="_blank">Continuous Integration</a>) you may probably need to add a reference to these files instead of pointing directly to the GAC, depending on what type of build server you are working with … <img style="border-bottom-style: none; border-left-style: none; border-top-style: none; border-right-style: none" class="wlEmoticon wlEmoticon-winkingsmile" alt="Winking smile" src="http://raffaeu.com/wp-content/uploads/2013/03/f0838511-0549-4876-a881-3aef8bab45e4wlEmoticon-winkingsmile_2.png" /></font>

<font color="#2e2e2e">Inside the folder Examples you will find a set of useful examples to start to learn TypeMock quickly but do not worry as I will go through all these examples in this series. </font>

<font color="#2e2e2e">If you want to make your experience with TypeMock easier and smoother, I kindly suggest you to download and install also TestDriven.NET or <a href="http://www.jetbrains.com/resharper/" target="_blank">Resharper</a> with <a href="http://www.gallio.org/" target="_blank">Gallio</a>. I personally use and love Resharper so you will find in this series all the reference examples pointing to the Resharper UI inside Visual Studio. The choice is up to you but I personally believe R# is the best tool so far for Visual Studio (IMHO)</font>

## Visual Studio integration

After the installation you can open Visual Studio and this is the surprise you will find in the IDE:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/c6d99adc-d002-4db0-8098-5c82ed17ae31Screen%20shot%202011-06-13%20at%2011.56.00%20PM_2.png" rel="lightbox[Tutorial01]"><img style="background-image: none; border-right-width: 0px; margin: 0px 8px 0px 0px; padding-left: 0px; padding-right: 0px; display: inline; float: left; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="Screen shot 2011-06-13 at 11.56.00 PM" border="0" alt="Screen shot 2011-06-13 at 11.56.00 PM" align="left" src="http://raffaeu.com/wp-content/uploads/2013/03/9e0378c8-7d6b-4cfb-8806-820ea4ea1178Screen%20shot%202011-06-13%20at%2011.56.00%20PM_thumb.png" width="260" height="198" /></a> You will find a new menu on Visual Studio called TypeMock; in this menu you can setup the license, the profiler to use with TypeMock and few other options for a better Visual Studio experience.

There aren’t a lot of other ways to easily configure TypeMock but we will see together how you can tackle each of the common tasks you may encounter while working with TypeMock.

From this menu you have also the easy option of enabling/disabling TypeMock at anytime so that you can or cannot work with it without the need to restarting Visual Studio every time (like you have to do with other plugins of Visual Studio).

## The Demo Project

I have created a very small project for this series of tutorials to show you how you can test every single layer of a .NET application using TypeMock to separate the dependencies. The structure of the demo project is in the following way:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/fec3dce1-0174-4cdb-acaf-26d704cafb06Screen%20shot%202011-06-19%20at%204.26.58%20PM_2.png" rel="lightbox[Tutorial01]"><img style="background-image: none; border-right-width: 0px; margin: 0px 3px 0px 0px; padding-left: 0px; padding-right: 0px; display: inline; float: left; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="Screen shot 2011-06-19 at 4.26.58 PM" border="0" alt="Screen shot 2011-06-19 at 4.26.58 PM" align="left" src="http://raffaeu.com/wp-content/uploads/2013/03/26181d70-4fb1-42fd-834c-f4673accbb9aScreen%20shot%202011-06-19%20at%204.26.58%20PM_thumb.png" width="251" height="260" /></a>

The project is composed by 4 different layers:

  1. TypeMockDemo: the project that contains the Domain Model of the tutorial
  2. TypeMockDemo.DataLayer: a data layer built around NHibernate 3.2
  3. TypeMockDemo.ServiceLayer: the service layer used to write the business logic around the domain and the data layer
  4. TypeMockDemo.UserInterface: an application developed using WPF 4.

 

 

For each project there is a corresponding “fixtures” project that includes all the fixtures related to the project. With fixture I mean “test” … <img style="border-bottom-style: none; border-left-style: none; border-top-style: none; border-right-style: none" class="wlEmoticon wlEmoticon-winkingsmile" alt="Winking smile" src="http://raffaeu.com/wp-content/uploads/2013/03/0c1a5626-a881-4b3c-8e55-dd17ac3248e5wlEmoticon-winkingsmile_2.png" />

## Tutorials and resources

Before starting to follow this series I kindly suggest you to have a look at the <a href="http://www.typemock.com" target="_blank">TypeMock</a> web site learning content, so that you will follow better my tutorials. As you know, I do not usually go too deep into a specific topic, so if you need to learn also what TDD is, I kindly suggest you to read also the following tutorials about TDD and testing in general.

**<u>TypeMock learning content:</u>**

  * Introduction to TypeMock [http://www.typemock.com/getting-started-step-1-set/](http://www.typemock.com/getting-started-step-1-set/ "http://www.typemock.com/getting-started-step-1-set/")
  * General articles and web casts [http://www.typemock.com/general-unit-testing-pages/](http://www.typemock.com/general-unit-testing-pages/ "http://www.typemock.com/general-unit-testing-pages/")
  * TypeMock <a href="http://docs.typemock.com/racer/##RacerHelp.chm/html/4734a51a-c37e-4d7b-aa7e-836a34fa7e23.htm" target="_blank">documentation</a>

**<u>TDD learning content:</u>**

  * TDD   
    [http://en.wikipedia.org/wiki/Test-driven_development](http://en.wikipedia.org/wiki/Test-driven_development "http://en.wikipedia.org/wiki/Test-driven_development")   
    [http://www.agiledata.org/essays/tdd.html](http://www.agiledata.org/essays/tdd.html "http://www.agiledata.org/essays/tdd.html")
  * Tutorials   
    This is a very complete introduction to TDD in .NET   
    [http://www.codeproject.com/KB/dotnet/tdd\_in\_dotnet.aspx](http://www.codeproject.com/KB/dotnet/tdd_in_dotnet.aspx "http://www.codeproject.com/KB/dotnet/tdd_in_dotnet.aspx")

 

So stay tuned and I’ll see you next Friday for the next part of this series.