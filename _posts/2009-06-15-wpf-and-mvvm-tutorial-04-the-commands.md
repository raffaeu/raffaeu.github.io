---
id: 260
title: WPF and MVVM tutorial 04, The Commands.
date: 2009-06-15T14:08:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=260
permalink: /archive/2009/06/15/wpf-and-mvvm-tutorial-04-the-commands.aspx
categories:
  - 'C#'
  - Design Pattern
  - WPF
---
In the previous posts we saw the basic of our project, <a href="http://blog.raffaeu.com/archive/2009/06/05/wpf-and-vmmv-tutorial-02-the-model.aspx" target="_blank">the model</a> (domain entities) and a <a href="http://blog.raffaeu.com/archive/2009/06/05/wpf-and-vmmv-tutorial-03-the-user-repository.aspx" target="_blank">simple repository</a>.

_Note: please note that the sample I wrote about an agnostic UnitOfWork is just to show you a simple pattern to handle L2S but it can be done better._

### The ViewModel.

What is the view model? Well the simplest explanation is enclosed in this definition: the view model should be the datacontext of our view. It should provide the commands, the observable collections used in the view and the error logic.

Before starting to view how to build the basic abstract class for the viewmodel I want to talk about the Relay Command and the Routed Command and their differences.

### Routed or Relay command?

I have found a useful <a href="http://joshsmithonwpf.wordpress.com/2008/03/18/understanding-routed-commands/" target="_blank">explanation here</a>, in the Josh Smith blog. Routed events are events designed to work in a tree of elements. When a user click the text over a button, the even travels over the tree until it will find the Click event of the chrome button.  This is how a button implements a routed events:

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> Button:ButtonBase
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:    <span style="color: #0000ff">static</span> Button()
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:    {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:       Button.ClickEvent = EventManager.RegistedRoutedEvent
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:          ("<span style="color: #8b0000">Click</span>", RoutingStrategy.Bubble,
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:           <span style="color: #0000ff">typeof</span>(RoutedEventHandler), <span style="color: #0000ff">typeof</span>(Button));
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:    }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:    <span style="color: #0000ff">public</span> RoutedEventHandler Click
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10:    {
</pre>


<pre style="background-color: #ffff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 11:       add { AddHandler(Button.ClickEvent,<span style="color: #0000ff">value</span>); }
</pre>


<pre style="background-color: #ffff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 12:       remove { RemoveHandler(Button.ClickEvent,<span style="color: #0000ff">value</span>); }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 13:    }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 14:    <span style="color: #0000ff">protected</span> <span style="color: #0000ff">override</span> <span style="color: #0000ff">void</span> 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 15:       OnMouseLeftButtonDown(MouseButtonEventArgs e){
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 16:       RaiseEvent(<span style="color: #0000ff">new</span> RoutedEventArgs(Button.ClickEvent,<span style="color: #0000ff">this</span>);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 17:    }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 18: }</pre>


<p>
  The <strong>commands</strong> in WPF represent a more independent action from their user interface. Also WPF and NET expose a default set of commands that we can easily handle in our application, like:
</p>


<p>
  Application command, ComponentCommand, MediaCommand, NavigationCommand and EditingCommand.
</p>


<p>
  They inherit all, from the <strong>ICommand</strong> interface. So for each command you want to implement, it should inherit from ICommand in this way:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: #region ICommand Members
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: <span style="color: #0000ff">public</span> <span style="color: #0000ff">bool</span> CanExecute(<span style="color: #0000ff">object</span> parameter) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:     <span style="color: #0000ff">return</span> _canExecute == <span style="color: #0000ff">null</span> ? <span style="color: #0000ff">true</span> : _canExecute(parameter);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7: <span style="color: #0000ff">public</span> <span style="color: #0000ff">event</span> EventHandler CanExecuteChanged {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:     add { CommandManager.RequerySuggested += <span style="color: #0000ff">value</span>; }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:     remove { CommandManager.RequerySuggested -= <span style="color: #0000ff">value</span>; }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 11: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 12: <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> Execute(<span style="color: #0000ff">object</span> parameter) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 13:     _execute(parameter);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 14: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 15: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 16: #endregion</pre>


<p>
  So it becomes simple to understand that we should have a View Model abstract class that contains an abstract implementation of a collection of ICommand. Then we can inherits each view model from this one!
</p>


<p>
  So our conclusion is that:
</p>


<blockquote>
  <p>
    The key difference is that RoutedCommand is an ICommand implementation that uses a RoutedEvent to route through the tree until a CommandBinding for the command is found, while RelayCommand does no routing and instead directly executes some delegate. In a M-V-VM scenario a RelayCommand (DelegateCommand in Prism) is probably the better choice all around.
  </p>
  
</blockquote>


<p>
  The final implementation of the Relay command should be something like:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial04TheCommands_C781/image.png" rel="lightbox[Tutorial4]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial04TheCommands_C781/image_thumb.png" width="195" height="260" /></a> 
</p>


<p>
  In the next tutorial we will see how to build the abstract layer for a basic view model with a basic collection of relay commands.
</p>


<p>
  Tags: <a href="http://technorati.com/tag/MVVM" rel="tag">MVVM</a> <a href="http://technorati.com/tag/Model View ViewModel" rel="tag">Model View ViewModel</a> <a href="http://technorati.com/tag/WPF" rel="tag">WPF</a>
</p>