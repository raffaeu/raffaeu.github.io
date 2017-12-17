---
id: 256
title: Inversion of Control with NET Framework.
date: 2009-07-07T18:10:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=256
permalink: /archive/2009/07/07/inversion-of-control-with-net-framework.aspx
categories:
  - 'C#'
  - Design Pattern
---
Today I want to break-out my series of posts about WPF to talk about an interesting design pattern. The Inversion of Control or Dependency Injection. You can find a clear definition at this address: <a href="http://martinfowler.com/articles/injection.html" target="_blank">Martin Fowler</a>.

### What is it the Dependency Injection?

The dependency injection is a way to inject some information and configuration inside an object, from another one. In this way we keep our object abstract and recyclable.

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/InversionofControlwithNETFramework_C7A8/image.png" rel="lightbox[IoC]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/InversionofControlwithNETFramework_C7A8/image_thumb.png" width="260" height="217" /></a> 

As you can see in this example, we have two concrete tasks, and each one has an **execute** command that is exposed by the interface **IBaseTask**. This will be our bridge from the interface and the concrete implementation.

### Manually Dependency Injection.

Now, if we would like to run our example, manually, we should write something like this code:

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: IBaseTask firstTask = <span style="color: #0000ff">new</span> RunTask();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: firstTask.Execute();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: IBaseTask secondTask = <span style="color: #0000ff">new</span> WalkTask();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: secondTask.Execute();</pre>


<p>
  And the code in each task should be something like that:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> WalkTask : IBaseTask {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:     #region IBaseTask Members
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> TaskName {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:         <span style="color: #0000ff">get</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:         <span style="color: #0000ff">set</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> Execute() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10:         Console.WriteLine("<span style="color: #8b0000">I am a Walk task</span>");
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 11:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 12: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 13:     #endregion
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 14: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 15: </pre>


<h3>
  Microsoft Unity for Dependency Injection.
</h3>


<p>
  Microsoft Unity is an open source project done by the Microsoft patterns and practice. It can be downloaded here: <a title="http://unity.codeplex.com/" href="http://unity.codeplex.com/">http://unity.codeplex.com/</a> and the actual version is the 1.2 present also in the enterprise library 4.1
</p>


<p>
  After you install it, you will have a folder with some .dlls that you have to reference into your solution.
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/InversionofControlwithNETFramework_C7A8/image_3.png" rel="lightbox[IoC]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/InversionofControlwithNETFramework_C7A8/image_thumb_3.png" width="260" height="200" /></a> 
</p>


<p>
  Now letâ€™s go back to our project and letâ€™s change the code in order to have Unity and not a concrete implementation of our interface.
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #008000">//Unity container</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: IUnityContainer container = <span style="color: #0000ff">new</span> UnityContainer();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: <span style="color: #008000">//Type association</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: container.RegisterType&lt;IBaseTask, WalkTask&gt;();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5: IBaseTask firstTask = container.Resolve&lt;IBaseTask&gt;();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6: firstTask.Execute();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7: </pre>


<p>
  This is the first step, but as you can notice, itâ€™s not so far from a concrete implementation. I mean, in this way we still have to <strong>procedural declare</strong> the type we want to convert to our interface.
</p>


<p>
  The next step will be to remove the type association and use a configuration file. First of all we need to declare in our <strong>app.config </strong>file the unity section:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">&lt;?</span>xml version="1.0" encoding="utf-8" <span style="color: #0000ff">?&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: <span style="color: #0000ff">&lt;</span><span style="color: #800000">configuration</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">configSections</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">section</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"Unity"</span> <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"Microsoft.Practices.Unity.Configuration.UnityConfigurationSection, 
</span></pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:              Microsoft.Practices.Unity.Configuration, Version=1.2.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"<span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">configSections</span><span style="color: #0000ff">&gt;</span></pre>


<p>
  Then we need to associate some type to our container. This is easy.
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">Unity</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">typeAliases</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:       <span style="color: #008000">&lt;!-- Lifetime manager types --&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:       <span style="color: #0000ff">&lt;</span><span style="color: #800000">typeAlias</span> <span style="color: #ff0000">alias</span>=<span style="color: #0000ff">"singleton"</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:            <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"Microsoft.Practices.Unity.ContainerControlledLifetimeManager,
</span></pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:                Microsoft.Practices.Unity" <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:       <span style="color: #0000ff">&lt;</span><span style="color: #800000">typeAlias</span> <span style="color: #ff0000">alias</span>=<span style="color: #0000ff">"external"</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:            <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"Microsoft.Practices.Unity.ExternallyControlledLifetimeManager,
</span></pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:                Microsoft.Practices.Unity" <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10:       <span style="color: #008000">&lt;!-- Custom types --&gt;</span>
</pre>


<pre style="background-color: #80ff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 11:       <span style="color: #0000ff">&lt;</span><span style="color: #800000">typeAlias</span> <span style="color: #ff0000">alias</span>=<span style="color: #0000ff">"myInterface"</span> <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"InversionOfControl.Model.IBaseTask, InversionOfControl"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #80ff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 12:     <span style="color: #0000ff">&lt;/</span><span style="color: #800000">typeAliases</span><span style="color: #0000ff">&gt;</span></pre>


<p>
  As you can see at the end, we are going to declare our IBaseTask interface. Remember always to declare the complete path otherwise Unity will search the class into the Unity namespace!!
</p>


<p>
  Now letâ€™s build a couple of custom map inside the XML file:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">containers</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:       <span style="color: #0000ff">&lt;</span><span style="color: #800000">container</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"firstContainer"</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:         <span style="color: #0000ff">&lt;</span><span style="color: #800000">types</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #80ff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:           <span style="color: #0000ff">&lt;</span><span style="color: #800000">type</span> <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"myInterface"</span> <span style="color: #ff0000">mapTo</span>=<span style="color: #0000ff">"InversionOfControl.Model.RunTask, InversionOfControl"</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"RunMapping"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #80ff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:           <span style="color: #0000ff">&lt;</span><span style="color: #800000">type</span> <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"myInterface"</span> <span style="color: #ff0000">mapTo</span>=<span style="color: #0000ff">"InversionOfControl.Model.WalkTask, InversionOfControl"</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"WalkMapping"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:         <span style="color: #0000ff">&lt;/</span><span style="color: #800000">types</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:       <span style="color: #0000ff">&lt;/</span><span style="color: #800000">container</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:     <span style="color: #0000ff">&lt;/</span><span style="color: #800000">containers</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">Unity</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10: <span style="color: #0000ff">&lt;/</span><span style="color: #800000">configuration</span><span style="color: #0000ff">&gt;</span></pre>


<p>
  As you can see, we are using our <strong>Alias</strong> to declare the interface and then we are giving a <strong>custom name</strong> the our mapping. Now we can go back to our code and do some fancy operations.
</p>


<p>
  Declare the configuration section and initialize the container:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: IUnityContainer container = <span style="color: #0000ff">new</span> UnityContainer();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: UnityConfigurationSection section = (UnityConfigurationSection)ConfigurationManager.GetSection("<span style="color: #8b0000">Unity</span>");
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: section.Containers["<span style="color: #8b0000">firstContainer</span>"].Configure(container);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: </pre>


<p>
  Then letâ€™s <strong>Inject</strong> a couple of objects inside our interface:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: IBaseTask firstTask = 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:    container.Resolve&lt;IBaseTask&gt;("<span style="color: #8b0000">RunMapping</span>");
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: firstTask.Execute();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: IBaseTask secondTask = 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:    container.Resolve&lt;IBaseTask&gt;("<span style="color: #8b0000">WalkMapping</span>");
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6: secondTask.Execute();</pre>


<p>
  Very easy right? In our example we are going to write in a procedural way which mapping we want to use, but of course this information should be retrieved at run-time from a Database or a serialized object.
</p>


<h3>
  Inject property and change values at run-time.
</h3>


<p>
  Until now our objects were exposing an execute method that was simply printing some fixed text in the console. But we have a <strong>name property</strong> so we should change the execute method with something like that:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> Execute() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:     Console.WriteLine
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:       ("<span style="color: #8b0000">I am a Run task. My name is: {0}</span>",
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:       TaskName);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6: </pre>


<p>
  Now we have two ways to initialize the TaskName property value without using a procedural code inside the program. First of all we have to say that the <strong>TaskName </strong>property of each concrete implementation has a dependency:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: [Dependency()]
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> TaskName {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:    <span style="color: #0000ff">get</span>;<span style="color: #0000ff">set</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: }</pre>


<p>
  And now we can change the configuration in this way:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">&lt;</span><span style="color: #800000">type</span> <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"myInterface"</span> <span style="color: #ff0000">mapTo</span>=<span style="color: #0000ff">"InversionOfControl.Model.WalkTask, InversionOfControl"</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"WalkMapping"</span> <span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">typeConfig</span> <span style="color: #ff0000">extensionType</span>=<span style="color: #0000ff">"Microsoft.Practices.Unity.Configuration.TypeInjectionElement,
</span></pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:                      Microsoft.Practices.Unity.Configuration"<span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">property</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"TaskName"</span>  <span style="color: #ff0000">propertyType</span>=<span style="color: #0000ff">"System.String"</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:       <span style="color: #0000ff">&lt;</span><span style="color: #800000">value</span> <span style="color: #ff0000">value</span>=<span style="color: #0000ff">"SecondTask"</span><span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:     <span style="color: #0000ff">&lt;/</span><span style="color: #800000">property</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">typeConfig</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8: <span style="color: #0000ff">&lt;/</span><span style="color: #800000">type</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9: </pre>


<p>
  We are assigning for the mapping called <strong>WalkMapping</strong> a value of â€œSecondTaskâ€ for the property <strong>TaskName</strong>. This solution is fine but itâ€™s still fixed. I mean, and if I would like to change the value at run-time and I cannot access the serialization of my object?
</p>


<p>
  Here is coming the second solution:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: container.Configure&lt;InjectedMembers&gt;()
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:    .ConfigureInjectionFor&lt;RunTask&gt;(
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:    <span style="color: #008000">//.ConfigureInjectionFor&lt;IBaseTask&gt;(</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:    <span style="color: #008000">//.ConfigureInjectionFor&lt;WalkTask&gt;(</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:       <span style="color: #0000ff">new</span> InjectionProperty("<span style="color: #8b0000">TaskName</span>", "<span style="color: #8b0000">12345</span>"));</pre>


<p>
  I put a couple of comments to show you what you can configure.
</p>


<p>
  I hope this short article will be useful to evaluate this amazing IoC framework.
</p>


<p>
  You can find the complete solution <a href="http://cid-ada7c80f0c2f057b.skydrive.live.com/self.aspx/Visual%20Studio%20Projects/InversionOfControl.zip" target="_blank">here, in my sky drive</a>
</p>


<p>
  Tags: <a href="http://technorati.com/tag/IoC" rel="tag">IoC</a> <a href="http://technorati.com/tag/Unity" rel="tag">Unity</a> <a href="http://technorati.com/tag/Dependency Injection" rel="tag">Dependency Injection</a>
</p>