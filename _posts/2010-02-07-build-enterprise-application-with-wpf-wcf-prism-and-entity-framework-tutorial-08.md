---
id: 221
title: Build enterprise application with WPF, WCF, Prism and Entity Framework. Tutorial 08.
date: 2010-02-07T14:09:35+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=221
permalink: /archive/2010/02/07/build-enterprise-application-with-wpf-wcf-prism-and-entity-framework-tutorial-08.aspx
categories:
  - 'C#'
  - Design Pattern
  - WCF
  - WPF
---
<p style="padding-bottom: 5px; padding-left: 5px; padding-right: 5px; background: #efefef; padding-top: 5px">
  <span style="font-size: 10pt"><span style="font-family: trebuchet ms; color: #333333"><strong>Update: </strong>source code and documentation are now available on <a href="http://www.codeplex.com"></a></span><span style="font-family: times new roman; color: #cc6600">CodePlex</span><span style="font-family: trebuchet ms; color: #333333"> at this address: <a href="http://prismtutorial.codeplex.com/"></a></span><span style="font-family: times new roman; color: #cc6600">http://prismtutorial.codeplex.com/</span><span style="font-family: trebuchet ms; color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-size: 10pt"><span style="font-family: trebuchet ms; color: #333333">Today we will build the skeleton of our application using the latest version of Prism, available in this section of MSDN: <a href="http://www.microsoft.com/downloads/details.aspx?FamilyID=fa07e1ce-ca3f-4b9b-a21b-e3fa10d013dd&DisplayLang=en" target="_blank"></a><span style="font-family: times new roman; color: #cc6600">Prism February 2009</span><span style="font-family: trebuchet ms; color: #333333">. One of the reasons that I didn&#8217;t write anything concrete until now is the fact that I knew that the new version of Prism was coming this month. <img border="0" alt="Smile" src="http://raffaeu.com/wp-content/uploads/2013/03/b1a4619e-a052-4e65-933d-8373b7eefa9asmiley-smile.gif" /> </span></span></span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #758d38; font-size: 12pt"><strong>The classic Hello World application. </strong></span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">As for every tutorial, is always better to start with the classic <strong>Hello World</strong> in order to understand, step by step, how Prism structures the various part of the UI. </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">Let&#8217;s open our VS Solution (remember that if you download the solution from my CodePlex repository the solution will contains already the entire code of each article) and let&#8217;s add a new project of type WPF application. I called Prism.Shell because this one will be our <strong>shell</strong> that will contain the <strong>bootstrapper</strong>. </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt"><span style="text-decoration: underline"><strong>Note about references</strong></span>. </span>
</p>

<p style="background: #efefef; margin-left: 2pt">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">In order to add the assemblies that I will mention in this and in the next tutorials you must download the latest version of Prism V2 and build the solutions so you will have all the assemblies you need. This is the structure of my Prism Framework: </span>
</p>

<p style="background: #efefef; margin-left: 2pt">
  <img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image_thumb6" border="0" alt="image_thumb6" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFPri_C7EF/image_thumb6.png" width="240" height="155" />
</p>

<p style="background: #efefef; margin-left: 2pt">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt"></span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">For practice I have already compiled all the required assemblies and added them to a solution folder in VS. This is what I have and what I reference in my projects: </span>
</p>

<p style="background: white">
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFPri_C7EF/image11.png" rel="lightbox[tutorial08]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image_thumb14" border="0" alt="image_thumb14" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFPri_C7EF/image_thumb14.png" width="174" height="240" /></a>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">Finally, remember also that you can open each Prism component and re-compile it, add new future and suggest new capabilities to the Microsoft team. They will appreciate for sure! <img border="0" alt="Smile" src="http://raffaeu.com/wp-content/uploads/2013/03/80415c25-7582-4e73-b839-3e9d0e0795desmiley-smile.gif" /> </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">So â€¦ coming back to our project this is what we need in our Shell: </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt"><span style="text-decoration: underline">References:</span> </span>
</p>

  1. <div style="background: white">
      <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt"><strong>Microsoft.Composite <br /></strong>This is the core of the composite framework </span>
    </div>

  2. <div style="background: white">
      <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt"><strong>Microsoft.Composite.Presentation <br /></strong>This assembly contains the components that target Prism for WPF </span>
    </div>

  3. <div style="background: white">
      <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt"><strong>Microsoft.Composite.UnityExtension <br /></strong>It&#8217;s a utility assembly for unity IoC container </span>
    </div>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #758d38; font-size: 12pt"><strong>The Shell XAML container. </strong></span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">Let&#8217;s add a new Windows Component from WPF template and the name will be Shell.xaml. Below the code I have added to the window in order to show 3 different regions. </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">1: </span><span style="color: blue"><</span><span style="color: maroon">Window</span><span style="color: #333333"> </span><span style="color: red">x</span><span style="color: #333333">:</span><span style="color: red">Class</span><span style="color: #333333">=</span><span style="color: blue">&#8220;Shell.Shell&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">2: </span><span style="color: red">xmlns</span><span style="color: #333333">=</span><span style="color: blue">&#8220;http://schemas.microsoft.com/winfx/2006/xaml/presentation&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">3: </span><span style="color: red">xmlns</span><span style="color: #333333">:</span><span style="color: red">x</span><span style="color: #333333">=</span><span style="color: blue">&#8220;http://schemas.microsoft.com/winfx/2006/xaml&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">4: </span><span style="color: red">Title</span><span style="color: #333333">=</span><span style="color: blue">&#8220;Hello World with Regions&#8221;</span><span style="color: #333333"> </span><span style="color: red">Height</span><span style="color: #333333">=</span><span style="color: blue">&#8220;500&#8221;</span><span style="color: #333333"> </span><span style="color: red">Width</span><span style="color: #333333">=</span><span style="color: blue">&#8220;500&#8221;></span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">5: </span><span style="color: blue"><</span><span style="color: maroon">DockPanel</span><span style="color: blue">></span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">6: </span><span style="color: blue"><</span><span style="color: maroon">ItemsControl</span><span style="color: #333333"> </span><span style="color: red">x</span><span style="color: #333333">:</span><span style="color: red">Name</span><span style="color: #333333">=</span><span style="color: blue">&#8220;HeaderRegion&#8221;</span><span style="color: #333333"> </span><span style="color: red">DockPanel</span><span style="color: #333333">.</span><span style="color: red">Dock</span><span style="color: #333333">=</span><span style="color: blue">&#8220;Top&#8221;</span><span style="color: #333333"> </span><span style="color: red">Height</span><span style="color: #333333">=</span><span style="color: blue">&#8220;50&#8221;></span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">7: </span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">8: </span><span style="color: blue"></</span><span style="color: maroon">ItemsControl</span><span style="color: blue">></span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">9: </span><span style="color: blue"><</span><span style="color: maroon">ItemsControl</span><span style="color: #333333"> </span><span style="color: red">x</span><span style="color: #333333">:</span><span style="color: red">Name</span><span style="color: #333333">=</span><span style="color: blue">&#8220;LeftRegion&#8221;</span><span style="color: #333333"> </span><span style="color: red">Width</span><span style="color: #333333">=</span><span style="color: blue">&#8220;100&#8221;</span><span style="color: #333333"> </span><span style="color: red">DockPanel</span><span style="color: #333333">.</span><span style="color: red">Dock</span><span style="color: #333333">=</span><span style="color: blue">&#8220;Left&#8221;></span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">10: </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">11: </span><span style="color: blue"></</span><span style="color: maroon">ItemsControl</span><span style="color: blue">></span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">12: </span><span style="color: blue"><</span><span style="color: maroon">ItemsControl</span><span style="color: #333333"> </span><span style="color: red">x</span><span style="color: #333333">:</span><span style="color: red">Name</span><span style="color: #333333">=</span><span style="color: blue">&#8220;MainRegion&#8221;</span><span style="color: #333333"> </span><span style="color: red">DockPanel</span><span style="color: #333333">.</span><span style="color: red">Dock</span><span style="color: #333333">=</span><span style="color: blue">&#8220;Bottom&#8221;></span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">13: </span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">14: </span><span style="color: blue"></</span><span style="color: maroon">ItemsControl</span><span style="color: blue">></span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">15: </span><span style="color: blue"></</span><span style="color: maroon">DockPanel</span><span style="color: blue">></span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">16: </span><span style="color: blue"></</span><span style="color: maroon">Window</span><span style="color: blue">></span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">We have added 3 <strong>ItemsControl</strong> inside a DockPanel in order to design a simple Outlook style layout with an header, a main container and a left navigation menu. </span>
</p>

<p style="background: white">
  <span style="font-size: 10pt"><span style="font-family: trebuchet ms; color: #333333">Now we need to extend this control using </span><a href="http://msdn.microsoft.com/en-us/library/ms749011.aspx" target="_blank"></a><span style="font-family: times new roman; color: #cc6600">the attached properties</span><span style="font-family: trebuchet ms; color: #333333"> in order to use the Prism region adapters. This is the code we must include in the xaml declaration: </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">1: xmlns:cal=&#8221;http://www.codeplex.com/CompositeWPF&#8221; </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">Now we are ready to change our xaml code in this way: </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">1: </span><span style="color: blue"><</span><span style="color: maroon">ItemsControl</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">2: </span><span style="color: red">x</span><span style="color: #333333">:</span><span style="color: red">Name</span><span style="color: #333333">=</span><span style="color: blue">&#8220;HeaderRegion&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">3: </span><span style="color: red">cal</span><span style="color: #333333">:</span><span style="color: red">RegionManager</span><span style="color: #333333">.</span><span style="color: red">RegionName</span><span style="color: #333333">=</span><span style="color: blue">&#8220;HeaderRegion&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">4: &#8230; </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">5: <</span><span style="color: red">ItemsControl</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">6: </span><span style="color: red">x</span><span style="color: #333333">:</span><span style="color: red">Name</span><span style="color: #333333">=</span><span style="color: blue">&#8220;LeftRegion&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">7: </span><span style="color: red">cal</span><span style="color: #333333">:</span><span style="color: red">RegionManager</span><span style="color: #333333">.</span><span style="color: red">RegionName</span><span style="color: #333333">=</span><span style="color: blue">&#8220;LeftRegion&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">8: &#8230; </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">9: <</span><span style="color: red">ItemsControl</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">10: </span><span style="color: red">x</span><span style="color: #333333">:</span><span style="color: red">Name</span><span style="color: #333333">=</span><span style="color: blue">&#8220;MainRegion&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">11: </span><span style="color: red">cal</span><span style="color: #333333">:</span><span style="color: red">RegionManager</span><span style="color: #333333">.</span><span style="color: red">RegionName</span><span style="color: #333333">=</span><span style="color: blue">&#8220;MainRegion&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">12: &#8230; </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">We said to Prism: &#8220;Hey when you load the Shell assign to each ItemsControl the corresponding Region using the attached property regionManager.RegionName&#8221;. </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #758d38; font-size: 12pt"><strong>The bootstrapper. </strong></span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">The bootstrapper is nothing more than the entry point of our application. He is in charge to resolve and charge the modules and assign them to the corresponding view for the initial setup. So we need to create a new class in our Shell project and call it Bootstrapper.cs. This is the code we need for our initial setup: </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">1: </span><span style="color: blue">protected</span><span style="color: #333333"> </span><span style="color: blue">override</span><span style="color: #333333"> IModuleCatalog GetModuleCatalog() { </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">2: ModuleCatalog catalog = </span><span style="color: blue">new</span><span style="color: #333333"> ModuleCatalog(); </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">3: </span><span style="color: blue">return</span><span style="color: #333333"> catalog; </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">4: } </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">This method should return an instance of the modules catalog used inside the application. What is the module catalog? The catalog contains all the modules we will use in our application. As we don&#8217;t have any module right now, this method returns an empty catalog. We will see in the future that we can load modules in the catalog also during the application execution. </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">1: </span><span style="color: blue">protected</span><span style="color: #333333"> </span><span style="color: blue">override</span><span style="color: #333333"> DependencyObject CreateShell() { </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">2: Shell shell = </span><span style="color: blue">new</span><span style="color: #333333"> Shell(); </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">3: shell.Show(); </span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">4: </span><span style="color: blue">return</span><span style="color: #333333"> shell; </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">5: } </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">The create shell method, is only creating a new instance of the main window, called Shell and showing it to the user. Let&#8217;s say that this is the first entry point. </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">Now we need to modify the app.xaml in order to startup our application using the Boostrapper. First of all remove this row from the xaml code: </span>
</p>

<p style="background: #ffff80">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">1: StartupUri=&#8221;</span><span style="color: darkred">Window1.xaml</span><span style="color: #333333">&#8220;> </span></span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">We don&#8217;t want anymore that the application will start using this window but we want to use the boostrapper. </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">Now we need to modify the app.xaml.cs in this way: </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">1: </span><span style="color: blue">protected</span><span style="color: #333333"> </span><span style="color: blue">override</span><span style="color: #333333"> </span><span style="color: blue">void</span><span style="color: #333333"> OnStartup(StartupEventArgs e) { </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">2: </span><span style="color: blue">base</span><span style="color: #333333">.OnStartup(e); </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">3: Bootstrapper bootstrapper = </span><span style="color: blue">new</span><span style="color: #333333"> Bootstrapper(); </span></span>
</p>

<p style="background: #ffff8a">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">4: bootstrapper.Run(); </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">5: } </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">As you can see, starting from now our application will request to the bootstrapper to load the first window and we won&#8217;t have any more dependency to the related main window. </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">You should be able to press F5 and run this part of the tutorial without any problem. </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #758d38; font-size: 12pt"><strong>Add custom content to the regions. </strong></span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">Until now we wrote the code to startup our prism application but we still need to create at least 3 views, in order to load some content to the corresponding regions. </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">We need for this a new &#8220;module&#8221; that in VS is nothing more than a Class Library Project. We need the following references in order to be able to use WPF and the composite framework in our assembly: </span>
</p>

<p style="background: white">
  <img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image_thumb22" border="0" alt="image_thumb22" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFPri_C7EF/image_thumb22.png" width="240" height="238" />
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">The first thing that every module will have is a &#8220;module class&#8221; that will implement the IModule contract from the composite framework. It&#8217;s pretty easy because this contract exposes only 1 method &#8220;void Initialize()&#8221;. The basic structure of the module class is something like this: </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">1: </span><span style="color: blue">public</span><span style="color: #333333"> </span><span style="color: blue">sealed</span><span style="color: #333333"> </span><span style="color: blue">class</span><span style="color: #333333"> HelloWorldModule : IModule { </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">2: </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">3: </span><span style="color: blue">private</span><span style="color: #333333"> </span><span style="color: blue">readonly</span><span style="color: #333333"> IRegionManager regionManager; </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">4: </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">5: </span><span style="color: blue">public</span><span style="color: #333333"> </span><span style="color: blue">void</span><span style="color: #333333"> Initialize() { </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">6: </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">7: } </span>
</p>

<p style="background: white">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">8: </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">9: </span><span style="color: blue">public</span><span style="color: #333333"> HelloWorldModule(IRegionManager regionManager) { </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">10: </span><span style="color: blue">this</span><span style="color: #333333">.regionManager = regionManager; </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">11: } </span>
</p>

<p style="background: white">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">12: } </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">13: </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">So we have declared a private <strong>IRegionManager</strong> that will represent the main region manager instantiated by the bootstrapper. As soon as the bootstrapper will load it will inject the region manager inside our module. Than we have created a constructor with dependency injection, this mean that as soon as the module will be declared it will receive a specific RegionManager class. Finally we have implemented the IRegionManager Initialize() method that will be used to register the views to the corresponding regions. </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">Now let&#8217;s go back to the shell application. We need now to add a &#8220;project reference&#8221; to the new module we have created. After that we need to add this line of code inside the Bootstrapper class: </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">1: </span><span style="color: blue">protected</span><span style="color: #333333"> </span><span style="color: blue">override</span><span style="color: #333333"> IModuleCatalog GetModuleCatalog() { </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">2: ModuleCatalog catalog = </span><span style="color: blue">new</span><span style="color: #333333"> ModuleCatalog() </span></span>
</p>

<p style="background: #ffff8a">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">3: .AddModule(</span><span style="color: blue">typeof</span><span style="color: #333333">(HelloWorldModule.HelloWorldModule)); </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">4: </span><span style="color: blue">return</span><span style="color: #333333"> catalog; </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">5: } </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">We said to the boostrapper that as soon as it will load it will need to add to the module catalog a new module, our HelloWorldModule class. If we try to run the app now it will run successfully but nothing will change in the UI because we still need to create the views. </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">What I want to show is the power of loading different views in different regions, so for this reason I am rendering a special textblock with some fancy shadows. This is the code and the final result: </span>
</p>

<p style="background: white">
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFPri_C7EF/image23.png" rel="lightbox[tutorial08]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image_thumb31" border="0" alt="image_thumb31" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFPri_C7EF/image_thumb31.png" width="240" height="117" /></a>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt"></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">1: </span><span style="color: blue"><</span><span style="color: maroon">Border</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">2: </span><span style="color: red">Margin</span><span style="color: #333333">=</span><span style="color: blue">&#8220;20&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">3: </span><span style="color: red">Padding</span><span style="color: #333333">=</span><span style="color: blue">&#8220;10&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">4: </span><span style="color: red">BorderThickness</span><span style="color: #333333">=</span><span style="color: blue">&#8220;3&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">5: </span><span style="color: red">CornerRadius</span><span style="color: #333333">=</span><span style="color: blue">&#8220;15&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">6: </span><span style="color: red">Background</span><span style="color: #333333">=</span><span style="color: blue">&#8220;White&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">7: </span><span style="color: red">BorderBrush</span><span style="color: #333333">=</span><span style="color: blue">&#8220;SteelBlue&#8221;></span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">8: </span><span style="color: blue"><</span><span style="color: maroon">Border.Effect</span><span style="color: blue">></span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">9: </span><span style="color: blue"><</span><span style="color: maroon">DropShadowEffect</span><span style="color: #333333"> </span><span style="color: red">Color</span><span style="color: #333333">=</span><span style="color: blue">&#8220;Gray&#8221;/></span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">10: </span><span style="color: blue"></</span><span style="color: maroon">Border.Effect</span><span style="color: blue">></span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">11: </span><span style="color: blue"></</span><span style="color: maroon">Border</span><span style="color: blue">></span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">12: </span><span style="color: blue"><</span><span style="color: maroon">Border</span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">13: </span><span style="color: red">Margin</span><span style="color: #333333">=</span><span style="color: blue">&#8220;20&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">14: </span><span style="color: red">Padding</span><span style="color: #333333">=</span><span style="color: blue">&#8220;10&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">15: </span><span style="color: red">BorderThickness</span><span style="color: #333333">=</span><span style="color: blue">&#8220;3&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">16: </span><span style="color: red">CornerRadius</span><span style="color: #333333">=</span><span style="color: blue">&#8220;15&#8221;></span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">17: </span><span style="color: blue"><</span><span style="color: maroon">TextBlock</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">18: </span><span style="color: red">Text</span><span style="color: #333333">=</span><span style="color: blue">&#8220;A custom header.&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">19: </span><span style="color: red">FontSize</span><span style="color: #333333">=</span><span style="color: blue">&#8220;24&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">20: </span><span style="color: red">TextWrapping</span><span style="color: #333333">=</span><span style="color: blue">&#8220;Wrap&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">21: </span><span style="color: red">Foreground</span><span style="color: #333333">=</span><span style="color: blue">&#8220;DarkSlateBlue&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">22: </span><span style="color: red">HorizontalAlignment</span><span style="color: #333333">=</span><span style="color: blue">&#8220;Center&#8221;</span><span style="color: #333333"> </span></span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">23: </span><span style="color: red">VerticalAlignment</span><span style="color: #333333">=</span><span style="color: blue">&#8220;Center&#8221;/></span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">24: </span><span style="color: blue"></</span><span style="color: maroon">Border</span><span style="color: blue">></span><span style="color: #333333"> </span></span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">Now, we need to add to our module 3 views using this effect in order to have this final result in our project: </span>
</p>

<p style="background: white">
  <img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image_thumb40" border="0" alt="image_thumb40" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFPri_C7EF/image_thumb40.png" width="240" height="110" />
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">Now, in the module class we need to tell to Prism where each view should be loaded in the shell region manager. The &#8216;declarative way&#8217; do to that it&#8217;s pretty simple: </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">1: </span><span style="color: blue">public</span><span style="color: #333333"> </span><span style="color: blue">void</span><span style="color: #333333"> Initialize() { </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">2: regionManager </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">3: .RegisterViewWithRegion(&#8220;</span><span style="color: darkred">HeaderRegion</span><span style="color: #333333">&#8220;, </span><span style="color: blue">typeof</span><span style="color: #333333">(Views.HeaderView)); </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">4: regionManager </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">5: .RegisterViewWithRegion(&#8220;</span><span style="color: darkred">LeftRegion</span><span style="color: #333333">&#8220;, </span><span style="color: blue">typeof</span><span style="color: #333333">(Views.LeftView)); </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">6: regionManager </span>
</p>

<p style="background: #fbfbfb">
  <span style="font-family: consolas; font-size: 10pt"><span style="color: #333333">7: .RegisterViewWithRegion(&#8220;</span><span style="color: darkred">MainRegion</span><span style="color: #333333">&#8220;, </span><span style="color: blue">typeof</span><span style="color: #333333">(Views.MainView)); </span></span>
</p>

<p style="background: white">
  <span style="font-family: consolas; color: #333333; font-size: 10pt">8: } </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">So for each view I assign the view to a corresponding region. The region name is the one we used in the attached property of the ItemsControl in the shell view. </span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">Now if you press F5 this will be your final result: </span>
</p>

<p style="background: white">
  <img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image_thumb43" border="0" alt="image_thumb43" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFPri_C7EF/image_thumb43.png" width="240" height="240" />
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt"></span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #758d38; font-size: 12pt"><strong>Conclusions. </strong></span>
</p>

<p style="background: white">
  <span style="font-family: trebuchet ms; color: #333333; font-size: 10pt">This is the &#8216;primordial&#8217; way to use Prism and to load 3 different views in 3 different regions. The next time we will see how many way Prism offers to load a module and the corresponding view, dynamically. </span>
</p>

<p style="background: white">
  <span style="font-size: 10pt"><span style="font-family: trebuchet ms; color: #333333">Tags: </span><a href="http://technorati.com/tag/WPF"></a></span><span style="font-family: times new roman; color: #cc6600">WPF</span><span style="font-family: trebuchet ms; color: #333333"> <a href="http://technorati.com/tag/Prism"></a></span><span style="font-family: times new roman; color: #cc6600">Prism</span><span style="font-family: trebuchet ms; color: #333333"> <a href="http://technorati.com/tag/Composite%20application"></a></span><span style="font-family: times new roman; color: #cc6600">Composite application</span><span style="font-family: trebuchet ms; color: #333333"> </span>
</p>