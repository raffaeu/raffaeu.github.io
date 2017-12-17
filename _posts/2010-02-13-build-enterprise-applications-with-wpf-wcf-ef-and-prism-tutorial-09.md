---
id: 213
title: Build enterprise applications with WPF, WCF, EF and Prism. Tutorial 09.
date: 2010-02-13T10:13:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=213
permalink: /archive/2010/02/13/build-enterprise-applications-with-wpf-wcf-ef-and-prism-tutorial-09.aspx
categories:
  - 'C#'
  - Design Pattern
  - WCF
  - WPF
---
Today we will see the modularity concept of Prism. Probably the best part of it, as it will allow us to build a real plug-and-play application where the main Shell wonâ€™t know anything about the modules.

### Discover and load the module.

What does it mean? As we saw in <a href="http://blog.raffaeu.com/archive/2010/01/31/ui-patterns-tutorials.-mvp-mvvm-and-composite-app-with-wpf.aspx" target="_blank">the previous article</a>, we have declared a reference in our Shell for the hello world module. In this way we force the Shell to be compiled including in the shell project a reference to the Hello World module. Fine, but what happen if we want to replace this module with a new version? We need to change the reference in the project and recompile it â€¦ Pretty ugly for a modular application doesnâ€™t it?

The basic concept in Prism is that we can **discover** on fly the available modules and load them â€¦ This is the workflow process of discovering and loading a module in Prism.

Â <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationswithWPFWCFEF_10D5C/prismmodule.png" rel="lightbox[tutorial09]"><img style="border-bottom: 0px; border-left: 0px; display: block; float: none; margin-left: auto; border-top: 0px; margin-right: auto; border-right: 0px" title="prism module" border="0" alt="prism module" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationswithWPFWCFEF_10D5C/prismmodule_thumb.png" width="260" height="202" /></a> 

There are different ways to load a module. As we saw in the previous article, the easiest way is to add a direct reference to the module in the Shell and load it in the bootstrapper:

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  1: ModuleCatalog catalog = <span style="color: #0000ff">new</span> ModuleCatalog()
</pre>


<pre style="background-color: #ffff8a; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  2:    .AddModule(<span style="color: #0000ff">typeof</span>(HelloWorldModule.HelloWorldModule));</pre>


<p>
  The second way is to add the module reference in the app.config and <strong>REMOVE</strong> the code in the bootstrapper. In this way we wonâ€™t need to <strong>recompile</strong> the module each time we will run the application in Visual Studio and we wonâ€™t need a direct reference to it. Letâ€™s see the code below. I have a added an app.config file in my shell project:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  1: <span style="color: #0000ff">&lt;?</span>xml version="1.0" encoding="utf-8" <span style="color: #0000ff">?&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  2: <span style="color: #0000ff">&lt;</span><span style="color: #800000">configuration</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  3:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">configSections</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffff8a; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  4:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">section</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"modules"</span>
</pre>


<pre style="background-color: #ffff8a; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  5:              <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"Microsoft.Practices.Composite.Modularity.ModulesConfigurationSection, Microsoft.Practices.Composite"</span><span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffff8a; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  6:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">configSections</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  7:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">modules</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  8:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">module</span> 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  9:       <span style="color: #ff0000">assemblyFile</span>=<span style="color: #0000ff">"Modules/HelloWorldModule.dll"</span> 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 10:       <span style="color: #ff0000">moduleType</span>=<span style="color: #0000ff">"HelloWorldModule.HelloWorldModule, HelloWorldModule"</span> 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 11:       <span style="color: #ff0000">moduleName</span>=<span style="color: #0000ff">"HelloWorldModule"</span><span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 12:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">modules</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 13: <span style="color: #0000ff">&lt;/</span><span style="color: #800000">configuration</span><span style="color: #0000ff">&gt;</span></pre>


<p>
  I am using the composite section in the app.config file to load a specific module. Now, in order to make this â€˜runnableâ€™ we need to:
</p>


<ul>
  <li>
    remove the reference in the Shell project 
  </li>
  
  
  <li>
    clean-up the code in the bootstrapper that loads the module 
  </li>
  
</ul>


<p>
  The shell project will use this code now to load the Module catalog:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  1: ModuleCatalog catalog = <span style="color: #0000ff">new</span> ConfigurationModuleCatalog();</pre>


<p>
  As you can see we are telling to Prism to look in the app.config and load the modules available there. In order to make the module available in the modules folder inside the bin folder of the shell you can play with MSBuild or a post-event action in VS. I copied this code from MSDN and added to the post event build action in the HelloWorldModule project properties:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  1: xcopy "<span style="color: #8b0000">$(TargetDir)*.*</span>" 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  2: "<span style="color: #8b0000">$(SolutionDir)Shell\bin\$(ConfigurationName)\Modules\" /Y</span></pre>


<p>
  Pretty cool! We can now work with our modules without breaking any change in the Shell project, and trust me, when you work on a team, this happens every day â€¦
</p>


<h3>
  Additional configuration options.
</h3>


<p>
  Of course the game is not done yet. We have a lot of options that we can configure in the app config in order to load modules on fly and to add dependencies from other modules. For example, letâ€™s say that Outlook canâ€™t open a view if the ribbon is not loaded, but the ribbon is on another module â€¦ <img src="http://raffaeu.com/wp-content/uploads/2013/03/e1b4e254-dc14-4b8b-8e3e-f3674e4748ccsmiley-wink.gif" border="0" alt="Wink" />
</p>


<p>
  <strong><u>Load on demand</u></strong>
</p>


<p>
  We can specify, for example, that we want to load on demand (by request) a specific module that we donâ€™t know. Pretty cool as this is how should work a real modular application. In order to do this we donâ€™t need anything more that this tag in the app.config:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  1: <span style="color: #0000ff">&lt;</span><span style="color: #800000">module</span> 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  2:    <span style="color: #ff0000">assemblyFile</span>=<span style="color: #0000ff">"Modules/HelloWorld.dll"</span> 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  3:    <span style="color: #ff0000">moduleType</span>=<span style="color: #0000ff">"HelloWorld, HelloWorld"</span> 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  4:    <span style="color: #ff0000">moduleName</span>=<span style="color: #0000ff">"HelloWorld"</span> 
</pre>


<pre style="background-color: #ffff8a; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  5:    <span style="color: #ff0000">InitializeMode</span>=<span style="color: #0000ff">"OnDemand"</span><span style="color: #0000ff">/&gt;</span></pre>


<p>
  Now that we setup the module to load â€œ<strong>onDemandâ€</strong> the module wonâ€™t be load in memory until we will instruct Prism to do that. This is a real plug and play concept, because without any dependencies we can load on demand part of our UI during the program execution. In Windows Form this canâ€™t simply be done!
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  1: moduleManager.LoadModule("<span style="color: #8b0000">HelloWorld</span>");</pre>


<p>
  There you go, in a simple call from any view we can ask for a specific module. The initialize method of the module will be call and our UI will receive the â€˜defaultâ€™ view of that module, injected in the shell or in another module â€¦ there are no limitations at all.
</p>


<p>
  Now we are ready to build the first cool module of our app, the main Ribbon bar.
</p>


<p>
  Tags: <a href="http://technorati.com/tag/WCF" rel="tag">WCF</a> <a href="http://technorati.com/tag/WPF" rel="tag">WPF</a> <a href="http://technorati.com/tag/Prism" rel="tag">Prism</a> <a href="http://technorati.com/tag/Composite application" rel="tag">Composite application</a>
</p>