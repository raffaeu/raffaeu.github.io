---
id: 258
title: WPF and MVVM tutorial 06, start up form.
date: 2009-06-17T13:31:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=258
permalink: /archive/2009/06/17/wpf-and-mvvm-tutorial-06-start-up-form.aspx
categories:
  - 'C#'
  - Design Pattern
  - WPF
---
Today we are going to create the start-up form of our project and use the first ViewModel to run the application logic.

The result we want to obtain will be:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial06startupform_BE33/image.png" rel="lightbox[Tutorial06]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial06startupform_BE33/image_thumb.png" width="260" height="245" /></a> 

The View Model for the Startup form.

In our project, letâ€™s go to the ViewModel section and create a new class. This class will inherit from the basic View Model class like the code below:

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> StartViewModel : ViewModel {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:    <span style="color: #0000ff">public</span> StartViewModel() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:    }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6: }</pre>


<p>
  Then we need to create 2 commands that we will then associate to two buttons in our XAML form. One will start the application and one will shut-down the application.
</p>


<p>
  This is the implementation we need to do in our ViewModel in order to create the Relay Command:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">public</span> ViewCommand startCommand;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: <span style="color: #0000ff">public</span> ViewCommand StartCommand {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:     <span style="color: #0000ff">get</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:         <span style="color: #0000ff">if</span> (startCommand == <span style="color: #0000ff">null</span>)
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:             startCommand = <span style="color: #0000ff">new</span> ViewCommand
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:                  (param =&gt; <span style="color: #0000ff">this</span>.StartApplication());
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:         <span style="color: #0000ff">return</span> startCommand;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:     }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10: </pre>


<p>
  Then we need a delegate for the relay command in this way:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">private</span> <span style="color: #0000ff">void</span> StartApplication() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:   NavigationActions.OpenCustomersView();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: </pre>


<p>
  The first XAML Form of our Application.
</p>


<p>
  I am not a guru in windows design, especially the fancy UI but the final project will have a startup form like this one:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial06startupform_BE33/image_3.png" rel="lightbox[Tutorial06]"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial06startupform_BE33/image_thumb_3.png" width="260" height="260" /></a> 
</p>


<p>
  First of all add a new XAML form into the UI layer and then letâ€™s see at the XAML code:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">&lt;</span><span style="color: #800000">Window</span> <span style="color: #ff0000">x</span>:<span style="color: #ff0000">Class</span>=<span style="color: #0000ff">"MVVM.WPFView.Start"</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:     <span style="color: #ff0000">xmlns</span>=<span style="color: #0000ff">"http://schemas.microsoft.com/winfx/2006/xaml/presentation"</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:     <span style="color: #ff0000">xmlns</span>:<span style="color: #ff0000">x</span>=<span style="color: #0000ff">"http://schemas.microsoft.com/winfx/2006/xaml"</span>
</pre>


<pre style="background-color: #ffff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:     <span style="color: #ff0000">xmlns</span>:<span style="color: #ff0000">vm</span>=<span style="color: #0000ff">"clr-namespace:MVVM.ViewModel;assembly=MVVM.ViewModel"</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:         <span style="color: #ff0000">Title</span>=<span style="color: #0000ff">"MVVM AdventureWorks Application."</span> 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:         <span style="color: #ff0000">Height</span>=<span style="color: #0000ff">"500"</span> <span style="color: #ff0000">Width</span>=<span style="color: #0000ff">"500"</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:         <span style="color: #ff0000">WindowStartupLocation</span>=<span style="color: #0000ff">"CenterScreen"</span> <span style="color: #ff0000">WindowState</span>=<span style="color: #0000ff">"Normal"</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:         <span style="color: #ff0000">WindowStyle</span>=<span style="color: #0000ff">"None"</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9: </pre>


<p>
  If you look at the line 4, we added a reference to our viewmodel namespace. In this way we can use in a declarative way the objects in the viewmodel namespace, directly into XAML (I personally hate to use procedural code into XAML!)
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">Window.DataContext</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:         <span style="color: #0000ff">&lt;</span><span style="color: #c71585">vm</span>:<span style="color: #800000">StartViewModel</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:     <span style="color: #0000ff">&lt;/</span><span style="color: #800000">Window.DataContext</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: </pre>


<p>
  Then we add as a resource, the StartViewModel view model into our Window. In this way when the window will load the XAML declaration will inform the CLR to load a default instance of our view model.
</p>


<p>
  Now letâ€™s view the two buttons:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">&lt;</span><span style="color: #800000">Button</span> <span style="color: #ff0000">Name</span>=<span style="color: #0000ff">"btnStart"</span> <span style="color: #ff0000">Margin</span>=<span style="color: #0000ff">"5"</span> 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: <span style="color: #ff0000">IsDefault</span>=<span style="color: #0000ff">"True"</span> 
</pre>


<pre style="background-color: #ffff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: <span style="color: #ff0000">Command</span>=<span style="color: #0000ff">"{Binding Path=StartCommand}"</span><span style="color: #0000ff">&gt;</span></pre>


<p>
  The binding will be the <strong>Relay Command</strong> we have previously created in the View model class. Easy right?
</p>


<p>
  Now, the next step will be to build the View model to show all the customers. Then we will build some specific command for the C.R.U.D. operations.
</p>


<p>
  Stay tuned!
</p>


<p>
  Tags: <a href="http://technorati.com/tag/MVVM" rel="tag">MVVM</a> <a href="http://technorati.com/tag/Model View ViewModel" rel="tag">Model View ViewModel</a> <a href="http://technorati.com/tag/WPF" rel="tag">WPF</a>
</p>