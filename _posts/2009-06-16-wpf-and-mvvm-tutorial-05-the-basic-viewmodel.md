---
id: 259
title: WPF and MVVM tutorial 05, The basic ViewModel.
date: 2009-06-16T18:11:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=259
permalink: /archive/2009/06/16/wpf-and-mvvm-tutorial-05-the-basic-viewmodel.aspx
categories:
  - 'C#'
  - Design Pattern
  - WPF
---
As we saw in the previous posts, a view model should be an abstract implementation of what the view needs to show about the model. We should implement an observable collection of something, we should implement an INotifyPropertyChanged interface and we should have a collection of <a href="http://blog.raffaeu.com/archive/2009/06/15/wpf-and-mvvm-tutorial-04-the-commands.aspx" target="_blank">RelayCommands</a>.

For these reasons, it simply to understand that we need a basic abstract ViewModel class, just to recycle some code.

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial05ThebasicViewModel_7961/image.png" rel="lightbox[Tutorial05]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial05ThebasicViewModel_7961/image_thumb.png" width="260" height="235" /></a> 

### The Basic View Model.

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">namespace</span> MVVM.ViewModel {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">abstract</span> <span style="color: #0000ff">class</span> ViewModel:INotifyPropertyChanged,IDisposable {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:         INavigationActions navigator;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:         <span style="color: #0000ff">public</span> ViewModel() {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:             navigator = Application.Current <span style="color: #0000ff">as</span> INavigationActions;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:             <span style="color: #0000ff">if</span> (navigator != <span style="color: #0000ff">null</span>) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:                 navigator.PropertyChanged += application_PropertyChanged;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10:             }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 11:         }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 12: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 13:         <span style="color: #0000ff">void</span> application_PropertyChanged(<span style="color: #0000ff">object</span> sender, PropertyChangedEventArgs e) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 14:             <span style="color: #0000ff">if</span> (<span style="color: #0000ff">string</span>.IsNullOrEmpty(e.PropertyName) || e.PropertyName == "<span style="color: #8b0000">View</span>")
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 15:                 OnPropertyChanged("<span style="color: #8b0000">View</span>");
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 16:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 17: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 18:         <span style="color: #0000ff">public</span> INavigationActions NavigationActions {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 19:             <span style="color: #0000ff">get</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 20:                 <span style="color: #0000ff">return</span> navigator;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 21:             }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 22:             <span style="color: #0000ff">set</span> {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 23:                 <span style="color: #0000ff">if</span> (navigator != <span style="color: #0000ff">value</span>) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 24:                     SetAction(<span style="color: #0000ff">value</span>);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 25:                     OnPropertyChanged("<span style="color: #8b0000">NavigationActions</span>");
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 26:                 }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 27:             }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 28:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 29: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 30:         <span style="color: #0000ff">protected</span> <span style="color: #0000ff">virtual</span> <span style="color: #0000ff">void</span> SetAction(INavigationActions <span style="color: #0000ff">value</span>) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 31:             <span style="color: #0000ff">if</span> (navigator != <span style="color: #0000ff">null</span>)
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 32:                 navigator.PropertyChanged -= application_PropertyChanged;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 33:             navigator = <span style="color: #0000ff">value</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 34:             <span style="color: #0000ff">if</span> (navigator != <span style="color: #0000ff">null</span>)
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 35:                 navigator.PropertyChanged += application_PropertyChanged;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 36:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 37: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 38:         #region INotifyPropertyChanged Members
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 39:         <span style="color: #808080">/// &lt;summary&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 40:         <span style="color: #808080">/// Raised when a property has a new value</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 41:         <span style="color: #808080">/// &lt;/summary&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 42:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">event</span> PropertyChangedEventHandler PropertyChanged;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 43:         <span style="color: #808080">/// &lt;summary&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 44:         <span style="color: #808080">/// Raise the event</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 45:         <span style="color: #808080">/// &lt;/summary&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 46:         <span style="color: #808080">/// &lt;param name="propertyName"&gt;Property name that has new value&lt;/param&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 47:         <span style="color: #0000ff">protected</span> <span style="color: #0000ff">virtual</span> <span style="color: #0000ff">void</span> OnPropertyChanged(<span style="color: #0000ff">string</span> propertyName) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 48:             PropertyChangedEventHandler handler = <span style="color: #0000ff">this</span>.PropertyChanged;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 49:             <span style="color: #0000ff">if</span> (handler != <span style="color: #0000ff">null</span>) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 50:                 var e = <span style="color: #0000ff">new</span> PropertyChangedEventArgs(propertyName);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 51:                 handler(<span style="color: #0000ff">this</span>, e);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 52:             }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 53:         }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 54:         #endregion
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 55: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 56:         #region IDisposable Members
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 57:         <span style="color: #808080">/// &lt;summary&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 58:         <span style="color: #808080">/// Implementation of the dispose method</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 59:         <span style="color: #808080">/// &lt;/summary&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 60:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> Dispose() {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 61:             <span style="color: #0000ff">this</span>.OnDispose();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 62:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 63:         <span style="color: #808080">/// &lt;summary&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 64:         <span style="color: #808080">/// The child class should implement a personal dispose procedure</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 65:         <span style="color: #808080">/// &lt;/summary&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 66:         <span style="color: #0000ff">protected</span> <span style="color: #0000ff">virtual</span> <span style="color: #0000ff">void</span> OnDispose() {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 67:             <span style="color: #008000">//do nothing because abstract</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 68:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 69: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 70:         #endregion
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 71:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 72: }</pre>


<p>
  A small summary of our code:
</p>


<ol>
  <li>
    An implementation of the INotifyPropertyChanged that we can use in the concrete views. 
  </li>
  
  
  <li>
    An implementation of the IDisposable in order to <strong>clean</strong> our objects like collections and repositories. 
  </li>
  
  
  <li>
    An INavigator interface to implement the navigation of our application. In this case I am using the <a href="http://www.codeproject.com/KB/WPF/CompositeWpfApp.aspx" target="_blank">navigator pattern for composite WPF applications</a>. Beware because this is <strong>my implementation</strong> for the navigation but it depends on how you design your app (multi-win, tab, MDI). 
  </li>
  
</ol>


<h3>
  The INavigator implementation.
</h3>


<p>
  The are thousands of ways to implement a navigator engine. The only common purpose in MVVM is that the View Model doesnâ€™t know the View but the View knows the View Model. So in our example which part of the View can interact with the application and doesnâ€™t need to know the View Model? The app.xaml file!
</p>


<p>
  My idea is this one:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial05ThebasicViewModel_7961/image_3.png" rel="lightbox[Tutorial05]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial05ThebasicViewModel_7961/image_thumb_3.png" width="260" height="197" /></a> 
</p>


<p>
  We will have a simple interface in the <strong>ViewModel namespace</strong> which will identify the <strong>navigation commands</strong> we want to execute:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">interface</span> INavigationActions:INotifyPropertyChanged 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:     <span style="color: #0000ff">void</span> OpenCustomersView();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:     <span style="color: #0000ff">void</span> OpenCustomerView();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:     <span style="color: #0000ff">void</span> OpenOrdersView();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:     <span style="color: #0000ff">void</span> OpenOrderView();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:     <span style="color: #0000ff">void</span> CloseCurrentView();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:     <span style="color: #0000ff">void</span> CloseApplication();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:     <span style="color: #0000ff">bool</span> QueryConfirmation(<span style="color: #0000ff">string</span> title, <span style="color: #0000ff">string</span> message);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10:     <span style="color: #0000ff">void</span> ShowError(<span style="color: #0000ff">string</span> title, <span style="color: #0000ff">string</span> message);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 11: }</pre>


<p>
  In this way we can call everything from our viewmodel that doesnâ€™t know the view.
</p>


<p>
  Now we just need to implement the view in the Application file in this way:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">public</span> partial <span style="color: #0000ff">class</span> App : Application,INavigationActions,INotifyPropertyChanged {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:     #region INavigationActions Members
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> OpenCustomersView() {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:         CustomersView customersView = <span style="color: #0000ff">new</span> CustomersView();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:         customersView.Show();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> OpenCustomerView() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 11:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 12: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 13:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> OpenOrdersView() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 14: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 15:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 16: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 17:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> OpenOrderView() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 18: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 19:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 20: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 21:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> CloseCurrentView() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 22: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 23:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 24: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 25:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> CloseApplication() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 26:         Application.Current.Shutdown();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 27:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 28: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 29:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">bool</span> QueryConfirmation(<span style="color: #0000ff">string</span> title, <span style="color: #0000ff">string</span> message) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 30:         <span style="color: #0000ff">return</span> MessageBox.Show(title, message, MessageBoxButton.YesNo, MessageBoxImage.Question) == MessageBoxResult.Yes;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 31:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 32: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 33:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> ShowError(<span style="color: #0000ff">string</span> title, <span style="color: #0000ff">string</span> message) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 34:         MessageBox.Show(title, message, MessageBoxButton.YesNo, MessageBoxImage.Error);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 35:     }</pre>


<p>
  And there we go!!
</p>


<p>
  Ops I forgot to mention that in the basic View Model we need to handle the INavigation
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">public</span> ViewModel() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:     navigator = Application.Current <span style="color: #0000ff">as</span> INavigationActions;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:     <span style="color: #0000ff">if</span> (navigator != <span style="color: #0000ff">null</span>) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:         navigator.PropertyChanged 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:         += application_PropertyChanged;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:     }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8: </pre>


<p>
  For the rest you need to wait the next tutorial!!
</p>


<p>
  Tags: <a href="http://technorati.com/tag/MVVM" rel="tag">MVVM</a> <a href="http://technorati.com/tag/Model View ViewModel" rel="tag">Model View ViewModel</a> <a href="http://technorati.com/tag/WPF" rel="tag">WPF</a>
</p>