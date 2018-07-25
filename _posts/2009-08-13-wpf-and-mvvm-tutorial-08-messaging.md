---
id: 250
title: WPF and MVVM tutorial 08, messaging.
date: 2009-08-13T16:19:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=250
permalink: /archive/2009/08/13/wpf-and-mvvm-tutorial-08-messaging.aspx
categories:
  - 'C#'
  - Design Pattern
  - WPF
---
Before continue our tutorial I want to take a break and talk about the messaging options. Also because the MVVM is a new **interpretation** of an old pattern, some of my friends are using the Mediator Pattern and today we must talk about that before building the Details view.

> _As it was requested by many people, here is the location of the source code on CodePlex: [www.codeplex.com/MVVM](http://www.codeplex.com/MVVM). Please refer there to any issue._

### Communication through the views.

Before starting we need to spend 5 minutes over the communication. The first solution we used was the **Navigator Pattern**. Actually what I do not like of the **Navigation Pattern** is that we are going to involve one object for the entire applicationâ€™s cycle. A different solution would be the **Mediator Pattern**.

Over the network there are some pretty cool frameworks like <a href="http://www.codeplex.com/CompositeWPF" target="_blank">Prism Project</a>, <a href="http://caliburn.codeplex.com/" target="_blank">Caliburn Framework</a> and more. For our purpose (understand and implement the MVVM pattern) we need something lighter but better than the simple navigator pattern.

I have found this **beta framework** that manage the communication between the views in a very nice and clean way: <a href="http://www.galasoft.ch/mvvm/getstarted/" target="_blank">MVVM Light Toolkit</a>. 

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial08CRUDoperations_A440/image.png" rel="lightbox[tutorial08]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial08CRUDoperations_A440/image_thumb.png" width="260" height="165" /></a> Using a framework, everything will be easier then. For example, we can implement in our **List View** a simple command that will pass a selected entity to a **Details View** in this way:

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #008000">//Creates detail window</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: DetalWindow view = <span style="color: #0000ff">new</span> EditDetalWindow();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: <span style="color: #008000">//Post ShowDetailWindow message</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: ShowDetailMessage message = <span style="color: #0000ff">new</span> ShowDetailMessage(<span style="color: #0000ff">this</span>, message.Content);
</pre>


<pre style="background-color: #ffffbb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5: Messenger.Default.Broadcast(message);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6: <span style="color: #008000">//Show Detail Window</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7: view.Show();</pre>


<h3>
  MVVM Light Toolkit, implementation with WPF.
</h3>


<p>
  This toolkit was created in order to facilitate the implementation of the MVVM pattern inside WPF and Silverlight applications. Actually it has a <strong>fancy</strong> setup that works great also with Visual Studio 2008. It installs inside VS:
</p>


<ul>
  <li>
    MVVM Application Template for Silverlight and WPF 
  </li>
  
  
  <li>
    ViewModel, View and ViewModelLocator class templates 
  </li>
  
  
  <li>
    Some cool code snippets 
  </li>
  
</ul>


<p>
  If you want to have a look in depth on how it works, please refer to this page: <a href="http://blog.galasoft.ch/archive/2009/06/14/mvvm-lsquolightrsquo-toolkit-for-wpf-and-silverlight.aspx">MVVM light toolkit for WPF and Silverlight</a>.
</p>


<p>
  So, coming back to our project, we just need to locate the .dll and add a reference to our ViewModel layer in this way:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial08CRUDoperations_A440/image_3.png" rel="lightbox[tutorial08]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial08CRUDoperations_A440/image_thumb_3.png" width="260" height="223" /></a> 
</p>


<p>
  Then we will be able to write some simple code like this one, directly in our existing ViewModels:
</p>


<p>
  <img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial08CRUDoperations_A440/image_4.png" width="260" height="133" /> 
</p>


<h3>
  Building a Sample Project with Galactic Toolkit.
</h3>


<p>
  After the installation you will have these 2 new templates inside Visual Studio:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial08CRUDoperations_A440/image_5.png" rel="lightbox[tutorial08]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial08CRUDoperations_A440/image_thumb_4.png" width="260" height="191" /></a>including some examples. 
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial08CRUDoperations_A440/image_6.png" rel="lightbox[tutorial08]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial08CRUDoperations_A440/image_thumb_5.png" width="218" height="260" /></a>
</p>


<p>
  The difference from what we did until now is in the communication that is handle by messaging. On our solution we were going to use the <strong>Navigator</strong> in order to send messages and to manipulate objects between the views.
</p>


<p>
  This is done in a different way by Galactic. 
</p>


<h3>
  First of all the communication with the user. 
</h3>


<p>
  We still have a static class but it is used in a real different way. You have to create a relay command inside the view model:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">public</span> RelayCommand ButtonCommand {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:     <span style="color: #0000ff">get</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:     <span style="color: #0000ff">private</span> <span style="color: #0000ff">set</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: }</pre>


<p>
  Then we need to assign the command, trough the binding process, to our view:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">&lt;</span><span style="color: #800000">Button</span> <span style="color: #ff0000">Content</span>=<span style="color: #0000ff">"{Binding ButtonContent}"</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:   <span style="color: #ff0000">Command</span>=<span style="color: #0000ff">"{Binding ButtonCommand}"</span></pre>


<p>
  And finally we can create a Relay Command instance and assign it to our ButtonCommand. Then we use a Message function to talk with the client, in this way:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">this</span>.ButtonCommand = <span style="color: #0000ff">new</span> RelayCommand(() =&gt;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:     var dialogMessage = <span style="color: #0000ff">new</span> DialogMessage(
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:         <span style="color: #0000ff">this</span>,
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:         <span style="color: #0000ff">typeof</span>(MainWindow),
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:         Resources.MessageBoxText,
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:         r =&gt; <span style="color: #0000ff">this</span>.DialogResult = r)
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:     {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:         Button = MessageBoxButton.YesNo,
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10:         Caption = Resources.MessageBoxCaption,
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 11:         DefaultResult = MessageBoxResult.No,
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 12:         Icon = MessageBoxImage.Question
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 13:     };
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 14: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 15:     Messenger.Default.Broadcast(dialogMessage);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 16: });
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 17: </pre>


<p>
  Pretty cool right?<br />
    <br />If instead of talking with the client, you need to open a new window and pass an entity to it you can do it also through the toolkit.
</p>


<p>
  You can view a simple here on <a href="http://blogs.ugidotnet.org/corrado/archive/2009/08/11/m-v-vm-viewmodels-intercommunication.aspx" target="_blank">Corrado Cavalli blog</a>.
</p>


<p>
  Now we are able in the next tutorial to write the C.R.U.D. view of our application.
</p>


<p>
  Stay tuned!!
</p>


<p>
  Tags: <a href="http://technorati.com/tag/MVVM" rel="tag">MVVM</a> <a href="http://technorati.com/tag/WPF" rel="tag">WPF</a> <a href="http://technorati.com/tag/Model View ViewModel" rel="tag">Model View ViewModel</a>
</p>