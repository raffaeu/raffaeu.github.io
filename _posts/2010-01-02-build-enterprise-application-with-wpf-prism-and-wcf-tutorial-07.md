---
id: 225
title: Build enterprise application with WPF, Prism and WCF. Tutorial 07.
date: 2010-01-02T12:31:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=225
permalink: /archive/2010/01/02/build-enterprise-application-with-wpf-prism-and-wcf-tutorial-07.aspx
categories:
  - 'C#'
  - Design Pattern
  - WCF
  - WPF
---
<blockquote style="border-bottom: gray 1px dotted; border-left: gray 1px dotted; background: #efefef; border-top: gray 1px dotted; border-right: gray 1px dotted">
  <p>
    <b>Update: </b>source code and documentation are now available on <a href="http://www.codeplex.com">CodePlex</a> at this address: <a href="http://prismtutorial.codeplex.com/">http://prismtutorial.codeplex.com/</a>
  </p>
</blockquote>

### Design of a Rich Interface using WPF.

WPF is a very powerful language and allows you to do everything you want. The only problem is that Microsoft didnâ€™t release yet a rich toolkit with all the controls we need and the XAML is not easy to learn.

Before starting to build a UI, usually, I follow these 3 fundamental steps.

1. Main layout of my application (MDI, Tab, Ribbon â€¦) 

2. Style of my application (Custom, Office style, Vista style) 

3. Resources available for free (Icons, themes, controls) 

After I have everything in my hands I can start to design the UI and match all the pieces of the puzzle. For example I can build the main environment (shell) and then create the single components (search control, navigation bar and so on).

### Sketch the UI with Microsoft Blend.

At this address, you can find an evaluation version of Microsoft Blend, a nice product for WPF designer that fully integrates with Visual Studio 2008/2010. Inside this amazing product, there a tool [called Sketchflow designer](http://www.microsoft.com/expression/products/Blend_Overview.aspx). You saw this tool in action in my [previous post](http://blog.raffaeu.com/archive/2009/11/08/build-enterprise-application-with-wpf-wcf-entity-framework-and-prism-once-more.aspx) or [at the MIX09](http://videos.visitmix.com/MIX09/c01f).

Why we should use a sketchflow designer instead of designing directly the UI? Because when you build an enterprise application that will be used byÂ  hundreds of users, itâ€™s fundamental to reflect the UI to what the users want. If they already know the Office 2007 UI and the ribbon concept, it wonâ€™t be difficult for them to move inside your new WPF application. If they work with a MDI application, you will need to think about it and redesign your idea of RIA. Remember that the user is the person that will use, buy, advertise you product. Itâ€™s the most important part of your software development process. So try to satisfy it before your ego â€¦ <img src="http://raffaeu.com/wp-content/uploads/2013/03/29cdbf0f-3ae3-4a9e-bf12-ad0b19c5f4a4smiley-wink.gif" border="0" alt="Wink" />

Anyway, I will leave you the pleasure of discovering the Sketchflow world. Here I just want to show you how our application will be designed and why.   
First of all the application will be divided in 3 main regions: the ribbon, the navigation and the content region. Like outlook 2007.

The layout will be something like this one:

Â <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFPrisma_B01D/NewPicture15.png" rel="lightbox"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="New Picture (15)" border="0" alt="New Picture (15)" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFPrisma_B01D/NewPicture15_thumb.png" width="239" height="182" /></a> 

As you can see, we will have 3 principal regions, plus a couple of additional components, like a status bar and a search context bar. 

Â <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFPrisma_B01D/NewPicture15_3.png" rel="lightbox"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="New Picture (15)" border="0" alt="New Picture (15)" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFPrisma_B01D/NewPicture15_thumb_3.png" width="239" height="182" /></a> 

### **Get the controls you need for free.**

Before buying thousands of dollars of UI controls, I usually step into Codeplex or Codeproject and I search what I am looking for. If I donâ€™t find anything, well I start to consider to use a different approach â€¦     <img src="http://raffaeu.com/wp-content/uploads/2013/03/c7d7b4e4-6ce4-425c-8098-b2263dfa63bcsmiley-smile.gif" border="0" alt="Smile" />  
You will wondering why I donâ€™t use something cool like Telerick, for example. What I donâ€™t like about the third party controls, especially when you pay them, is the fact that you donâ€™t give you access to the source code, and you have to pay thousands of dollars for a datagrid. Also XAML is so powerful that after you will handle it, you wonâ€™t need any third party control. Trust me!

Anyway, this is the list of controls that we will need for our application:

1. A Ribbon 

2. An Outlook style navigation bar 

3. A tabbed â€œMDIâ€ container. 

About the ribbon, finally Microsoft has released a full license to use and work with the Microsoft Office 2007 Ribbon using XAML. In order to use it you must sign and agree to the license at this address: [Codeplex Office 2007 WPF Ribbon project](http://www.codeplex.com/wikipage?ProjectName=wpf&title=WPF%20Ribbon%20Preview).

About the Outlook navigation bar there are 2 good choices for free. The first one is the [article on CodeProject](http://www.codeproject.com/KB/WPF/XAML_OutlookBar.aspx) that explains you, in a step by step tutorial, how to build an outlook 2007 navigation bar starting with a tabbed control. Very cool.

If you are lazy, there is also this set of free controls, that I use, available on CodePlex. [Composite Application Contribution](http://www.codeplex.com/CompositeWPFContrib). Very well done and useful if you plan to work with Prism.

Then we need a Tabbed MDI container. We have already one in WPF that we can try to customize, and this is what we will do in the next articles. Otherwise you can use the one provided in the Composite WPF Contribution or there is [this good article](http://www.switchonthecode.com/tutorials/the-wpf-tab-control-inside-and-out) that explains how to fully customize a tabbed control.

Then we can also have the need of using a grid, right? Well if you really think you need one, you can use the [XCeed data grid](http://xceed.com/pages/TopMenu/Products/ProductSearch.aspx?Lang=EN-CA), for free, otherwise you can move to the [WPF control toolkit](http://wpfcontroltoolkit.codeplex.com/) or the [WPF contribution](http://www.codeplex.com/wpfcontrib), two nice open source projects that deliver some nice and useful controls.

### Ready to go?

Now, from the next article we will start to build our applicationâ€™s skeleton, so letâ€™s try to be ready with everything youâ€™ll need.

Download all these controls and add a reference into your Visual Studio Toolbox. Then agree and download the Office 2007 Ribbon project for WPF. Download the [latest version of Prism](http://www.codeplex.com/CompositeWPF/) and install also the [Enterprise Library 4.1](http://www.codeplex.com/CompositeWPF/), required to use Prism.

Also, if you have time, try to have a look at this [series of articles](http://msdn.microsoft.com/en-us/library/ms754130.aspx) and [web cast](http://windowsclient.net/learn/videos_wpf.aspx) about WPF fundamentals. In this way you will better understand my steps.

Enjoy this tutorial and stay tuned for the next one!

Tags: [WCF](http://technorati.com/tag/WCF) [Prism](http://technorati.com/tag/Prism) [WPF](http://technorati.com/tag/WPF) [Composite application](http://technorati.com/tag/Composite%20application)