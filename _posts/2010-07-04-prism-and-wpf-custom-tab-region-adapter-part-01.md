---
id: 196
title: Prism and WPF. Custom Tab region adapter. Part 01.
date: 2010-07-04T11:25:29+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=196
permalink: /archive/2010/07/04/prism-and-wpf-custom-tab-region-adapter-part-01.aspx
categories:
  - 'C#'
  - Design Pattern
  - WPF
---
<a href="http://blog.raffaeu.com/archive/2010/07/04/wpf-and-prism-tab-region-adapter-part-02.aspx" target="_blank">The second part of this tutorial is here. Enjoy!</a>

One thing that I really hate when I started working on Prism was the fact that the developers didn’t provide a nice tab region adapter. Well, the items adapter provided in Prism fits perfectly with the tab control, but if you use it “as is” it pretty sucks. 

I am pretty sure that as soon as you will release your first Prism “Visual Studio style” Prism application your manager will ask you for a cook tab like the one below:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/69bb09ea-f676-44a1-8444-f9d7c887c78aimage_2.png" rel="lightbox"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/11f949ff-156b-40df-98f4-9f6f5b4323c2image_thumb.png" width="483" height="56" /></a> 

This is an add-in of VS2010. As you can see it allows you to: 1) close the current active tab, 2) identify the tabs that have changes with a red dot, 3) use different colors in order to identify the active tab and the different types of files opened.

Another cool tab system, also present in VS 2010 is the one that splits the XAML code from the designer IDE, look here:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/5f442a4c-0c94-463b-ad20-073619236e35image_4.png" rel="lightbox"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/1c7d38a6-db39-4b53-9b39-1bc60843aa1dimage_thumb_1.png" width="209" height="51" /></a> 

I simply love this style of a non–squared tab, it remembers me Chrome browser.

### How Prism region adapter works?

First of all let’s have a look on how we create a new tab in a tab region adapter, using the default Prism settings. What you usually do is to add a view to a region and that’s it … The code below shows you how to define a tab region adapter and then how to add a new tab on it. Pretty straightforward.

**First of all we initialize the bootstrapper in the Shell project.**

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:a505486f-2e00-4da9-ba2b-f93768883f7e" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      The Shell bootstrapper
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">protected</span> <span style="color:#0000ff">override</span> System.Windows.<span style="color:#2b91af">DependencyObject</span> CreateShell()
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">var</span> shell = Container.Resolve<<span style="color:#2b91af">MainWindow</span>>();
        </li>
        <li style="background: #f3f3f3">
              shell.Show();
        </li>
        <li>
              <span style="color:#0000ff">return</span> shell;
        </li>
        <li style="background: #f3f3f3">
          }
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
          <span style="color:#0000ff">protected</span> <span style="color:#0000ff">override</span> <span style="color:#2b91af">IModuleCatalog</span> GetModuleCatalog()
        </li>
        <li>
          {
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">var</span> catalog = <span style="color:#0000ff">new</span> <span style="color:#2b91af">ModuleCatalog</span>()
        </li>
        <li>
                  .AddModule(<span style="color:#0000ff">typeof</span>(<span style="color:#2b91af">TabModule</span>));
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">return</span> catalog;
        </li>
        <li>
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

**Then we prepare the Shell XAML Window to include the regions we need.**

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:74dc3ab3-b1df-4e69-80d0-4a963c7be862" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Shell XAML Window
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#a31515"></span><span style="color:#0000ff"><</span><span style="color:#a31515">ItemsControl</span><span style="color:#ff0000"> Grid.Row</span><span style="color:#0000ff">=&#8221;0&#8243;</span><span style="color:#ff0000"> Grid.Column</span><span style="color:#0000ff">=&#8221;0&#8243;</span><span style="color:#ff0000"> Grid.ColumnSpan</span><span style="color:#0000ff">=&#8221;2&#8243;</span>
        </li>
        <li style="background: #f3f3f3">
                       <span style="color:#ff0000"> cal</span><span style="color:#0000ff">:</span><span style="color:#ff0000">RegionManager.RegionName</span><span style="color:#0000ff">=&#8221;HeaderRegion&#8221;</span><span style="color:#ff0000"> Name</span><span style="color:#0000ff">=&#8221;HeaderRegion&#8221;></span>
        </li>
        <li>
              <span style="color:#a31515"></span>
        </li>
        <li style="background: #f3f3f3">
          <span style="color:#a31515"></span><span style="color:#0000ff"></</span><span style="color:#a31515">ItemsControl</span><span style="color:#0000ff">></span>
        </li>
        <li>
          <span style="color:#a31515"></span><span style="color:#0000ff"><</span><span style="color:#a31515">TabControl</span><span style="color:#ff0000"> Grid.Row</span><span style="color:#0000ff">=&#8221;1&#8243;</span><span style="color:#ff0000"> Grid.Column</span><span style="color:#0000ff">=&#8221;1&#8243;</span>
        </li>
        <li style="background: #f3f3f3">
                     <span style="color:#ff0000"> cal</span><span style="color:#0000ff">:</span><span style="color:#ff0000">RegionManager.RegionName</span><span style="color:#0000ff">=&#8221;TabRegion&#8221;</span><span style="color:#ff0000"> Name</span><span style="color:#0000ff">=&#8221;TabRegion&#8221;></span>
        </li>
        <li>
              <span style="color:#a31515"></span>
        </li>
        <li style="background: #f3f3f3">
          <span style="color:#a31515"></span><span style="color:#0000ff"></</span><span style="color:#a31515">TabControl</span><span style="color:#0000ff">></span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

And then, in the module, we just assign each view to the specific region in the shell.

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:0c08f82a-6b22-4ab4-a915-9a7f7f59dc4a" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Module initialization
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">sealed</span> <span style="color:#0000ff">class</span> <span style="color:#2b91af">TabModule</span> : <span style="color:#2b91af">IModule</span>
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">private</span> <span style="color:#2b91af">IRegionManager</span> regionManager = <span style="color:#0000ff">null</span>;
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
              <span style="color:#0000ff">public</span> TabModule(<span style="color:#2b91af">IRegionManager</span> regionManager)
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">this</span>.regionManager = regionManager;
        </li>
        <li style="background: #f3f3f3">
              }
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">public</span> <span style="color:#0000ff">void</span> Initialize()
        </li>
        <li>
              {
        </li>
        <li style="background: #f3f3f3">
                  regionManager
        </li>
        <li>
                      .AddToRegion(<span style="color:#a31515">&#8220;TabRegion&#8221;</span>, <span style="color:#0000ff">new</span> <span style="color:#2b91af">FirstView</span>())
        </li>
        <li style="background: #f3f3f3">
                      .AddToRegion(<span style="color:#a31515">&#8220;TabRegion&#8221;</span>, <span style="color:#0000ff">new</span> <span style="color:#2b91af">SecondView</span>());
        </li>
        <li>
              }
        </li>
        <li style="background: #f3f3f3">
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

And this is the ugly result you get …

<a href="http://raffaeu.com/wp-content/uploads/2013/03/28712ab4-a0ee-4379-87a8-a4717959a363image_6.png" rel="lightbox"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/b4488785-862a-4dae-bd44-466a60ff8e8dimage_thumb_2.png" width="320" height="127" /></a> 

As you can see the tab doesn’t know how to render the tab item because we didn’t specify any style.

Now in order to fix this there are two simple solutions. The first one is to create a custom region adapter and override the method on adding a region and provide all the info you need to the region adapter. This is cool especially if you are using third party controls. The second way is the Raf’s way, as I don’t use at all third party controls for the simple reason that as a blogger, I don’t have money … [:)]

### Everything starts with the Dependency Property.

XAML is cool especially because it allows you to extend it as much as you want, so why don’t we start to consider its powerful engine before going mad adopting strange solutions?? So let’s build a simple .dll project that will be our “model” for the TabItem style. This model represents our properties to say that: 1) A view has a title, 2) A View can be or cannot be closed, 3) A view has changes that must be saved.

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:9eb0ac16-539e-41fe-9392-b3f264510563" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Model for a tab item
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">sealed</span> <span style="color:#0000ff">class</span> <span style="color:#2b91af">TabModel</span> : <span style="color:#2b91af">DependencyObject</span>
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">public</span> <span style="color:#0000ff">string</span> Title
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">get</span> { <span style="color:#0000ff">return</span> (<span style="color:#0000ff">string</span>)GetValue(TitleProperty); }
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#0000ff">set</span> { SetValue(TitleProperty, <span style="color:#0000ff">value</span>); }
        </li>
        <li>
              }
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
              <span style="color:#008000">// Using a DependencyProperty as the backing store for Title.  This enables animation, styling, binding, etc&#8230;</span>
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">readonly</span> <span style="color:#2b91af">DependencyProperty</span> TitleProperty =
        </li>
        <li>
                  <span style="color:#2b91af">DependencyProperty</span>.Register(<span style="color:#a31515">&#8220;Title&#8221;</span>, <span style="color:#0000ff">typeof</span>(<span style="color:#0000ff">string</span>), <span style="color:#0000ff">typeof</span>(<span style="color:#2b91af">TabModel</span>));
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
              <span style="color:#0000ff">public</span> <span style="color:#0000ff">bool</span> CanClose
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">get</span> { <span style="color:#0000ff">return</span> (<span style="color:#0000ff">bool</span>)GetValue(CanCloseProperty); }
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#0000ff">set</span> { SetValue(CanCloseProperty, <span style="color:#0000ff">value</span>); }
        </li>
        <li>
              }
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
              <span style="color:#008000">// Using a DependencyProperty as the backing store for CanClose.  This enables animation, styling, binding, etc&#8230;</span>
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">readonly</span> <span style="color:#2b91af">DependencyProperty</span> CanCloseProperty =
        </li>
        <li>
                  <span style="color:#2b91af">DependencyProperty</span>.Register(<span style="color:#a31515">&#8220;CanClose&#8221;</span>, <span style="color:#0000ff">typeof</span>(<span style="color:#0000ff">bool</span>), <span style="color:#0000ff">typeof</span>(<span style="color:#2b91af">TabModel</span>));
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
              <span style="color:#0000ff">public</span> <span style="color:#0000ff">bool</span> IsModified
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">get</span> { <span style="color:#0000ff">return</span> (<span style="color:#0000ff">bool</span>)GetValue(IsModifiedProperty); }
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#0000ff">set</span> { SetValue(IsModifiedProperty, <span style="color:#0000ff">value</span>); }
        </li>
        <li>
              }
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
              <span style="color:#008000">// Using a DependencyProperty as the backing store for IsModified.  This enables animation, styling, binding, etc&#8230;</span>
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">readonly</span> <span style="color:#2b91af">DependencyProperty</span> IsModifiedProperty =
        </li>
        <li>
                  <span style="color:#2b91af">DependencyProperty</span>.Register(<span style="color:#a31515">&#8220;IsModified&#8221;</span>, <span style="color:#0000ff">typeof</span>(<span style="color:#0000ff">bool</span>), <span style="color:#0000ff">typeof</span>(<span style="color:#2b91af">TabModel</span>));
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

Now we need just to create a ViewModel which represents a View rendered in a tab control. Of course you should have the entire infrastructure of your ViewModel toolkit but in this sample we don’t really need it! This is the code:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/78a90c93-2586-4d25-ac87-9286ff13852bimage_8.png" rel="lightbox[TabItem]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/ac3b8fe8-75da-4851-b6f4-d23ca29e46e6image_thumb_3.png" width="500" height="164" /></a>

Now that everything is in place we need two more pieces of code. The first is a ViewModel for each view which implements also the TabItem model, the code below is just for the FirstView:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:0bd204c0-f426-4b8d-9faa-39f25904ebba" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Concrete ViewModel for TabItem
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">sealed</span> <span style="color:#0000ff">class</span> <span style="color:#2b91af">FirstViewModel</span> : <span style="color:#2b91af">TabViewModel</span>
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">public</span> FirstViewModel()
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">this</span>.TabModel = <span style="color:#0000ff">new</span> <span style="color:#2b91af">TabModel</span>
        </li>
        <li style="background: #f3f3f3">
                  {
        </li>
        <li>
                      Title = <span style="color:#a31515">&#8220;First View.&#8221;</span>,
        </li>
        <li style="background: #f3f3f3">
                      CanClose = <span style="color:#0000ff">true</span>,
        </li>
        <li>
                      IsModified = <span style="color:#0000ff">false</span>
        </li>
        <li style="background: #f3f3f3">
                  };
        </li>
        <li>
              }
        </li>
        <li style="background: #f3f3f3">
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

And the very easy xaml:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:979d2746-d8b8-4a56-b0de-061db1d6448d" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      FirstView vm binding
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
                  <span style="color:#ff0000"> xmlns</span><span style="color:#0000ff">:</span><span style="color:#ff0000">vm</span><span style="color:#0000ff">=&#8221;clr-namespace:PrismCustomModule.ViewModels&#8221;</span>
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#ff0000"> mc</span><span style="color:#0000ff">:</span><span style="color:#ff0000">Ignorable</span><span style="color:#0000ff">=&#8221;d&#8221;</span>
        </li>
        <li>
                  <span style="color:#ff0000"> d</span><span style="color:#0000ff">:</span><span style="color:#ff0000">DesignHeight</span><span style="color:#0000ff">=&#8221;300&#8243;</span><span style="color:#ff0000"> d</span><span style="color:#0000ff">:</span><span style="color:#ff0000">DesignWidth</span><span style="color:#0000ff">=&#8221;300&#8243;></span>
        </li>
        <li style="background: #f3f3f3">
          <span style="color:#a31515"></span><span style="color:#0000ff"><</span><span style="color:#a31515">UserControl.DataContext</span><span style="color:#0000ff">></span>
        </li>
        <li>
              <span style="color:#a31515"></span><span style="color:#0000ff"><</span><span style="color:#a31515">vm</span><span style="color:#0000ff">:</span><span style="color:#a31515">FirstViewModel</span><span style="color:#0000ff"> /></span>
        </li>
        <li style="background: #f3f3f3">
          <span style="color:#a31515"></span><span style="color:#0000ff"></</span><span style="color:#a31515">UserControl.DataContext</span><span style="color:#0000ff">></span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

### Customize the TabItem using styles.

The first step is very easy, just go in the Shell project and add a Resource dictionary that will be then referenced in the Shell Window. Create a custom style for the tab container and specify how to bind the header text with the TabItem model we created previously:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:853f1494-a810-4d41-a2a5-0bcf60d7e968" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Dictionary
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff"><</span><span style="color:#a31515">ResourceDictionary</span><span style="color:#ff0000"> xmlns</span><span style="color:#0000ff">=&#8221;http://schemas.microsoft.com/winfx/2006/xaml/presentation&#8221;</span>
        </li>
        <li style="background: #f3f3f3">
                             <span style="color:#ff0000"> xmlns</span><span style="color:#0000ff">:</span><span style="color:#ff0000">x</span><span style="color:#0000ff">=&#8221;http://schemas.microsoft.com/winfx/2006/xaml&#8221;></span>
        </li>
        <li>
              <span style="color:#a31515"></span><span style="color:#0000ff"><</span><span style="color:#a31515">Style</span><span style="color:#ff0000"> TargetType</span><span style="color:#0000ff">=&#8221;{</span><span style="color:#a31515">x</span><span style="color:#0000ff">:</span><span style="color:#a31515">Type</span><span style="color:#ff0000"> TabItem}</span><span style="color:#0000ff">&#8220;</span><span style="color:#ff0000"> x</span><span style="color:#0000ff">:</span><span style="color:#ff0000">Key</span><span style="color:#0000ff">=&#8221;HeaderStyle&#8221;></span>
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#a31515"></span><span style="color:#0000ff"><</span><span style="color:#a31515">Setter</span><span style="color:#ff0000"> Property</span><span style="color:#0000ff">=&#8221;Header&#8221;</span>
        </li>
        <li>
                         <span style="color:#ff0000"> Value</span><span style="color:#0000ff">=&#8221;{</span><span style="color:#a31515">Binding</span><span style="color:#ff0000"> RelativeSource</span><span style="color:#0000ff">={</span><span style="color:#a31515">RelativeSource</span><span style="color:#ff0000"> Self}</span><span style="color:#0000ff">,</span>
        </li>
        <li style="background: #f3f3f3">
                             <span style="color:#ff0000"> Path</span><span style="color:#0000ff">=Content.DataContext.TabModel.Title}&#8221; /></span>
        </li>
        <li>
              <span style="color:#a31515"></span><span style="color:#0000ff"></</span><span style="color:#a31515">Style</span><span style="color:#0000ff">></span>
        </li>
        <li style="background: #f3f3f3">
          <span style="color:#0000ff"></</span><span style="color:#a31515">ResourceDictionary</span><span style="color:#0000ff">></span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

So far so good, now we just customize the TabContainer in the shell and that’s the final result:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:2d9a35f9-26d9-4ecd-b61d-3076e38eae93" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Code Snippet
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#a31515"></span><span style="color:#0000ff"><</span><span style="color:#a31515">TabControl</span><span style="color:#ff0000"> Grid.Row</span><span style="color:#0000ff">=&#8221;1&#8243;</span><span style="color:#ff0000"> Grid.Column</span><span style="color:#0000ff">=&#8221;1&#8243;</span>
        </li>
        <li style="background: #f3f3f3">
                     <span style="color:#ff0000"> cal</span><span style="color:#0000ff">:</span><span style="color:#ff0000">RegionManager.RegionName</span><span style="color:#0000ff">=&#8221;TabRegion&#8221;</span><span style="color:#ff0000"> Name</span><span style="color:#0000ff">=&#8221;TabRegion&#8221;</span>
        </li>
        <li>
                     <span style="color:#ff0000"> ItemContainerStyle</span><span style="color:#0000ff">=&#8221;{</span><span style="color:#a31515">StaticResource</span><span style="color:#ff0000"> HeaderStyle}</span><span style="color:#0000ff">&#8220;></span>
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#a31515"></span>
        </li>
        <li>
          <span style="color:#a31515"></span><span style="color:#0000ff"></</span><span style="color:#a31515">TabControl</span><span style="color:#0000ff">></span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

<a href="http://raffaeu.com/wp-content/uploads/2013/03/17f2ceb9-ac7d-4214-93c2-f3fde9d89415image_10.png" rel="lightbox"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/cb706a12-b9e7-4580-aef2-e85abb540ab1image_thumb_4.png" width="262" height="112" /></a> 

Pretty good and because the TabItem model is a property that raises changes in the ViewModel, we can communicate with the user changing this property “on fly” when the tab is open. We can also use it to activate a specific tab, adding a behavior to the tab style and more. But the tab header still sucks, and a lot! I mean I really don’t like it. So here we come with Blend 4 and Design 4.

There are a lot of tutorials on the web about Blend and Design. I came out with a final result pretty minimal but functional, this one using this tutorial: [http://stackoverflow.com/questions/561931/how-to-create-trapezoid-tabs-in-wpf-tab-control](http://stackoverflow.com/questions/561931/how-to-create-trapezoid-tabs-in-wpf-tab-control "http://stackoverflow.com/questions/561931/how-to-create-trapezoid-tabs-in-wpf-tab-control")

Another interesting project is AvalonDock on Codeplex. They just resemble the VS2010 style just using XAML code. This is the link for the style they used for the tab header [http://avalondock.codeplex.com/SourceControl/changeset/view/60974#1218433](http://avalondock.codeplex.com/SourceControl/changeset/view/60974#1218433 "http://avalondock.codeplex.com/SourceControl/changeset/view/60974#1218433") and this is the result:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/643d4d24-6757-462b-b9d5-d2797a566698image_12.png" rel="lightbox[TabItem]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/7a646815-a1dd-47e6-9c9c-6a8ddfdd24aaimage_thumb_5.png" width="404" height="49" /></a> 

Next time I will show you how to add a custom behavior to the tab in order to broadcast with prism the closing event, so that prism can kill the corresponding view …

[;)] Stay tuned!