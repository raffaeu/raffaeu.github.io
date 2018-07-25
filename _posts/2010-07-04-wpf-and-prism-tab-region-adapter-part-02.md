---
id: 183
title: WPf and Prism Tab Region Adapter, Part 02.
date: 2010-07-04T18:02:39+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=183
permalink: /archive/2010/07/04/wpf-and-prism-tab-region-adapter-part-02.aspx
categories:
  - 'C#'
  - Design Pattern
  - WPF
---
<a href="http://blog.raffaeu.com/archive/2010/07/04/prism-and-wpf-custom-tab-region-adapter.aspx" target="_blank">As we saw in the previous post</a>, it’s pretty complicate to create a custom design in WPF to override the default style of the TabControl, but it’s pretty simple to extend the behavior of it. 

As a senior dev, I usually don’t like to: 1) touch what is already working, 2) reinvent the wheel just to write the same code twice. For that there is the refactoring process, at most!

So, let’s start by the requirement we had in the previous post, we need to emulate the VS IDE in our applications, that’s it! We also saw that Prism has the RegionAdapter, so now we just need a cool control. Well there is the Avalon Dock project that is open source, really well done, flexible and ready for WPF 4. So let’s use it for our purposes. The final result should be something like this:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/ad984be7-787f-4691-b351-234cccea82a2image_2.png" rel="lightbox"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/e9e6dceb-baba-46c5-bf4d-2311170bce5bimage_thumb.png" width="420" height="78" /></a> 

<a href="http://avalondock.codeplex.com/" target="_blank">Avalon dock</a> is crazy powerful and allows you to build a complete system of docking and modal windows into your WPF application. But before doing that you need to write a custom region adapter for it!

So, this is the basic concept of a custom Region Adapter in Prism. You create you custom adapter class by inheriting from RegionAdapterBase<T> in the following way:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:f43e8c34-53db-4c62-bb6f-534aee7ed9a5" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Region adapter for Avalon Dock
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">sealed</span> <span style="color:#0000ff">class</span> <span style="color:#2b91af">AvalonRegionAdapter</span> : <span style="color:#2b91af">RegionAdapterBase</span><<span style="color:#2b91af">DocumentPane</span>>
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">public</span> AvalonRegionAdapter(<span style="color:#2b91af">IRegionBehaviorFactory</span> factory)
        </li>
        <li style="background: #f3f3f3">
                  : <span style="color:#0000ff">base</span>(factory)
        </li>
        <li>
              {
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

 

Avalon dock has a lot of future, and you should create a Region adapter able to attach any type of dock view. In my case I will use only the Tab region adapter that is called DocumentPane. Now the RegionAdapterBase requires that you implement three methods:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:e112627c-1d9e-46d4-8590-811017121a7d" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Create region
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">protected</span> <span style="color:#0000ff">override</span> <span style="color:#2b91af">IRegion</span> CreateRegion()
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">return</span> <span style="color:#0000ff">new</span> <span style="color:#2b91af">AllActiveRegion</span>();
        </li>
        <li style="background: #f3f3f3">
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

The create region which specify what base adapter you want to use. In this case we want to handle any type of view added to this adapter, like an ItemsContainer or a TabControl.

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:75b57c4d-956c-4329-a850-9824fe317c32" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Adapt
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">protected</span> <span style="color:#0000ff">override</span> <span style="color:#0000ff">void</span> Adapt(<span style="color:#2b91af">IRegion</span> region, <span style="color:#2b91af">DocumentPane</span> regionTarget)
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              region.Views.CollectionChanged += <span style="color:#0000ff">delegate</span>(<span style="color:#2b91af">Object</span> sender, <span style="color:#2b91af">NotifyCollectionChangedEventArgs</span> e)
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  OnViewsCollectionChanged(sender, e, region, regionTarget);
        </li>
        <li style="background: #f3f3f3">
              };
        </li>
        <li>
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

Then we override the adapt method. This method is called once so in my case, because I have a DocumentPane I will then listen for the event Views.CollectionChanged. In this way I know every time if a view has been added or removed from the region. 

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:c40b0db1-9ff5-47fa-b1e4-3505d057ba31" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Code Snippet
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">private</span> <span style="color:#0000ff">void</span> OnViewsCollectionChanged(<span style="color:#0000ff">object</span> sender, <span style="color:#2b91af">NotifyCollectionChangedEventArgs</span> e, <span style="color:#2b91af">IRegion</span> region, <span style="color:#2b91af">DocumentPane</span> regionTarget)
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">if</span> (e.Action == <span style="color:#2b91af">NotifyCollectionChangedAction</span>.Add)
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#008000">//Add content panes for each associated view. </span>
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#0000ff">foreach</span> (<span style="color:#0000ff">object</span> item <span style="color:#0000ff">in</span> e.NewItems)
        </li>
        <li>
                  {
        </li>
        <li style="background: #f3f3f3">
                      <span style="color:#2b91af">UIElement</span> view = item <span style="color:#0000ff">as</span> <span style="color:#2b91af">UIElement</span>;
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
                      <span style="color:#0000ff">if</span> (view != <span style="color:#0000ff">null</span>)
        </li>
        <li>
                      {
        </li>
        <li style="background: #f3f3f3">
                          <span style="color:#2b91af">DockableContent</span> newContentPane = <span style="color:#0000ff">new</span> <span style="color:#2b91af">DockableContent</span>();
        </li>
        <li>
                          newContentPane.Content = item;
        </li>
        <li style="background: #f3f3f3">
                          <span style="color:#008000">//if associated view has metadata then apply it.</span>
        </li>
        <li>
                          newContentPane.Title = view.GetType().ToString();
        </li>
        <li style="background: #f3f3f3">
                          <span style="color:#008000">//When contentPane is closed remove the associated region </span>
        </li>
        <li>
                          newContentPane.Closed += (contentPaneSender, args) =>
        </li>
        <li style="background: #f3f3f3">
                          {
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
                          };
        </li>
        <li>
                          regionTarget.Items.Add(newContentPane);
        </li>
        <li style="background: #f3f3f3">
                          newContentPane.Activate();
        </li>
        <li>
                      }
        </li>
        <li style="background: #f3f3f3">
                  }
        </li>
        <li>
              }
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">else</span>
        </li>
        <li>
              {
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#0000ff">if</span> (e.Action == <span style="color:#2b91af">NotifyCollectionChangedAction</span>.Remove)
        </li>
        <li>
                  {
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
                  }
        </li>
        <li style="background: #f3f3f3">
              }
        </li>
        <li>
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

Now, here there are two major steps. First we want to know if the item has been added or removed from the collection. If it’s added we create a new DockableContent and we set the Content to our view. Then we need to set a couple of properties like Title and name. In my case I am just adding the view, we will see later how to implement our TabModel property. What we can do then is to attach a listener to the close event of this tab. Why? Because when avalon will close a dock document we need also to destroy the corresponding view.

Then the second part is when the regionAdapter has a request of closing a view. We want to destroy the corresponding tab control.

Now go back a little bit and change the code in this way:</p> 

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:51250967-e7df-49e5-9d88-6ac1dfc6a1f2" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Code Snippet
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#2b91af">TabViewModel</span> viewModel = ((<span style="color:#2b91af">UserControl</span>)view).DataContext <span style="color:#0000ff">as</span> <span style="color:#2b91af">TabViewModel</span>;
        </li>
        <li style="background: #f3f3f3">
          <span style="color:#0000ff">if</span> (view != <span style="color:#0000ff">null</span>)
        </li>
        <li>
          {
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#2b91af">DockableContent</span> newContentPane = <span style="color:#0000ff">new</span> <span style="color:#2b91af">DockableContent</span>();
        </li>
        <li>
              newContentPane.Content = item;
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">if</span> (viewModel != <span style="color:#0000ff">null</span>)
        </li>
        <li>
              {
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#2b91af">Image</span> img = <span style="color:#0000ff">new</span> <span style="color:#2b91af">Image</span>();
        </li>
        <li>
                  img.Source = <span style="color:#0000ff">new</span> <span style="color:#2b91af">BitmapImage</span>(<span style="color:#0000ff">new</span> <span style="color:#2b91af">Uri</span>(<span style="color:#a31515">@&#8221;Resources/Alerts.png&#8221;</span>, <span style="color:#2b91af">UriKind</span>.Relative));
        </li>
        <li style="background: #f3f3f3">
                  newContentPane.Title = viewModel.TabModel.Title;
        </li>
        <li>
                  newContentPane.IsCloseable = viewModel.TabModel.CanClose;
        </li>
        <li style="background: #f3f3f3">
                  newContentPane.Icon = img.Source;
        </li>
        <li>
              }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

It’s a little bit dirty but what we are trying to do here is to cast the View.DataContext to a type TabViewModel. It it’s the right type, as NET won’t throw an exception but simply returns an empty instance … we will populate the tab controls with our info.

The final result is this one:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/1b665f99-d5cb-4c3f-9e0a-890c4fc6fbffSNAGHTML388d63e.png" rel="lightbox"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="SNAGHTML388d63e" border="0" alt="SNAGHTML388d63e" src="http://raffaeu.com/wp-content/uploads/2013/03/ba984552-64cb-45ff-856e-850fa5e11bd2SNAGHTML388d63e_thumb.png" width="484" height="199" /></a>

The first can’t be closed and the second one can be, also we added a special icon to the context menu. More than that, this is still a WPF controls where you can apply your custom style. That’s it!

Ops, this is the new code in the MainView, of course:</p> 

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:85e9550f-03fb-4d77-9b3e-512e74aa11f4" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      XAML Avalon Dock adapter
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#a31515"></span><span style="color:#0000ff"><</span><span style="color:#a31515">ad</span><span style="color:#0000ff">:</span><span style="color:#a31515">DockingManager</span><span style="color:#ff0000"> Grid.Column</span><span style="color:#0000ff">=&#8221;1&#8243;</span><span style="color:#ff0000"> Grid.Row</span><span style="color:#0000ff">=&#8221;1&#8243; ></span>
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#a31515"></span><span style="color:#0000ff"><</span><span style="color:#a31515">ad</span><span style="color:#0000ff">:</span><span style="color:#a31515">DocumentPane</span><span style="color:#ff0000"> cal</span><span style="color:#0000ff">:</span><span style="color:#ff0000">RegionManager.RegionName</span><span style="color:#0000ff">=&#8221;TabRegion&#8221;</span><span style="color:#ff0000"> Name</span><span style="color:#0000ff">=&#8221;TabRegion&#8221;></span>
        </li>
        <li>
                  <span style="color:#a31515"></span><span style="color:#0000ff"><</span><span style="color:#a31515">ad</span><span style="color:#0000ff">:</span><span style="color:#a31515">DockableContent</span><span style="color:#ff0000"> Title</span><span style="color:#0000ff">=&#8221;Some title&#8221;></span>
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
                  <span style="color:#a31515"></span><span style="color:#0000ff"></</span><span style="color:#a31515">ad</span><span style="color:#0000ff">:</span><span style="color:#a31515">DockableContent</span><span style="color:#0000ff">></span>
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#a31515"></span><span style="color:#0000ff"></</span><span style="color:#a31515">ad</span><span style="color:#0000ff">:</span><span style="color:#a31515">DocumentPane</span><span style="color:#0000ff">></span>
        </li>
        <li>
          <span style="color:#a31515"></span><span style="color:#0000ff"></</span><span style="color:#a31515">ad</span><span style="color:#0000ff">:</span><span style="color:#a31515">DockingManager</span><span style="color:#0000ff">></span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

**<u>Final Note:</u>** As you can see building a custom region adapter is easy but you must know the control behind that, the one that will act as a region adapter. When you know that part you can play as you want. For example I have a region adapter for the ToolBar so every time you load a view, I pass the corresponding Toolbar to the main toolbar region. Same for the ribbon and for the Outlook bar.