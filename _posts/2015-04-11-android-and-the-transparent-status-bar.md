---
id: 664
title: Android and the transparent status bar
date: 2015-04-11T18:32:26+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=664
permalink: /archive/:year/:month/:day/:title/
dsq_needs_sync:
  - "1"
categories:
  - Android
  - Mobile
---
With the introduction of <a href="http://www.google.com/design/spec/material-design/introduction.html" target="_blank">Google Material Design</a> we also got a new status bar design and we can choose for three different layouts:

  * Leave the status bar as is (usually black background and white foreground) 
  * Change the color of the status bar using a different **tone** 
  * Make the status bar transparent 

The picture below shows the three different solutions provided with the Material Design guidelines:

**A) Classic Status Bar**

<img src="http://material-design.storage.googleapis.com/publish/v_2/material_ext_publish/0Bx4BSt6jniD7XzcyLTZxU2pMTGc/layout_structure_system_color3.png" width="240" height="127" />

**B) Colored Status Bar**

<img src="http://material-design.storage.googleapis.com/publish/v_2/material_ext_publish/0Bx4BSt6jniD7STVfdi10U1hkaXc/layout_structure_system_color1.png" width="240" height="127" />

**C) Transparent Status Bar**

<img src="http://material-design.storage.googleapis.com/publish/v_2/material_ext_publish/0Bx4BSt6jniD7NWl0bmRRTnNZeHM/layout_structure_system_color2.png" width="240" height="127" />

In this post I want to finally give a working solution that allows you to achieve all this variations of the status. _Except for the first solution which is the default layout of Android, so if you don’t want to comply to the Material Design Guidelines just leave the status bar black colored_.

## Change the Color of the StatusBar

So the first solution we want to try here is to change the color of the status bar. I have a main layout with a **Toolbar** component in it and the Toolbar component has a background color like the following:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Android-and-the-transparent-status-bar_1146A/image_thumb.png" width="244" height="35" />](http://blog.raffaeu.com/wp-content/uploads/Android-and-the-transparent-status-bar_1146A/image.png)

So according Material Design my Status Bar should be colored using the following 700 tone variation:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Android-and-the-transparent-status-bar_1146A/image_thumb_3.png" width="244" height="34" />](http://blog.raffaeu.com/wp-content/uploads/Android-and-the-transparent-status-bar_1146A/image_3.png)

If you are working with Material Design **only** and Android Lollipop this is quite easy to accomplish, just set the proper attribute inside the Material Theme Style(v21) XML file as following:

<pre class="csharpcode"><span class="rem">&lt;!-- This is the color of the Toolbar --&gt;</span>
<span class="kwrd">&lt;</span><span class="html">item</span> <span class="attr">name</span><span class="kwrd">="colorPrimary"</span><span class="kwrd">&gt;</span>@color/primary<span class="kwrd">&lt;/</span><span class="html">item</span><span class="kwrd">&gt;</span>
<span class="rem">&lt;!-- This is the color of the Status bar --&gt;</span>
<span class="kwrd">&lt;</span><span class="html">item</span> <span class="attr">name</span><span class="kwrd">="colorPrimaryDark"</span><span class="kwrd">&gt;</span>@color/primary_dark<span class="kwrd">&lt;/</span><span class="html">item</span><span class="kwrd">&gt;</span>
<span class="rem">&lt;!-- The Color of the Status bar --&gt;</span>
<span class="kwrd">&lt;</span><span class="html">item</span> <span class="attr">name</span><span class="kwrd">="statusBarColor"</span><span class="kwrd">&gt;</span>@color/primary_dark<span class="kwrd">&lt;/</span><span class="html">item</span><span class="kwrd">&gt;</span></pre>

Unfortunately this solutions does not make your status bar transparent, so if you have a Navigation Drawer the final result will look a bit odd compared to the real Material Design guidelines, like the following one:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Android-and-the-transparent-status-bar_1146A/image_thumb_4.png" width="244" height="110" />](http://blog.raffaeu.com/wp-content/uploads/Android-and-the-transparent-status-bar_1146A/image_4.png)

As you can see the Status Bar simply covers the Navigation Drawer giving a final odd layout. But with this simple solution you can change your status bar color but only for Lollipop systems.

> In Android KitKat **you cannot change the color of the status bar** except if you use the following solution because only in Lollipop Google introduced the attribute statuBarColor

## Make the StatusBar transparent

A second solution is to make the Status Bar transparent. This is easy to achieve by using the following XML attributes in your Styles.xml and Styles(v21).xml:

<pre class="csharpcode"><span class="rem">&lt;!-- Make the status bar traslucent --&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">style</span> <span class="attr">name</span><span class="kwrd">="AppTheme"</span> <span class="attr">parent</span><span class="kwrd">="AppTheme.Base"</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">item</span> <span class="attr">name</span><span class="kwrd">="android:windowTranslucentStatus"</span><span class="kwrd">&gt;</span>true<span class="kwrd">&lt;/</span><span class="html">item</span><span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">style</span><span class="kwrd">&gt;</span></pre>

But with only this solution in place we get another odd result where the Toolbar moves behind the status bar and get cut like the following screenshot:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Android-and-the-transparent-status-bar_1146A/image_thumb_5.png" width="316" height="55" />](http://blog.raffaeu.com/wp-content/uploads/Android-and-the-transparent-status-bar_1146A/image_5.png)

So first of all we need to inform the Activity that we need to add some **padding** to our toolbar and the padding should be the size of the status bar, **which is completely different from one device to another**. So how can we achieve that is quite simple. First we get the status bar height with this function:

<pre class="csharpcode"><span class="rem">// A method to find height of the status bar</span>
<span class="kwrd">public</span> <span class="kwrd">int</span> getStatusBarHeight() {
    <span class="kwrd">int</span> result = 0;
    <span class="kwrd">int</span> resourceId = getResources().getIdentifier(<span class="str">"status_bar_height"</span>, <span class="str">"dimen"</span>, <span class="str">"android"</span>);
    <span class="kwrd">if</span> (resourceId &gt; 0) {
        result = getResources().getDimensionPixelSize(resourceId);
    }
    <span class="kwrd">return</span> result;
}</pre>

Then in our **OnCreate** method we specify the padding of the Toolbar with the following code:

<pre class="csharpcode">@Override
<span class="kwrd">protected</span> <span class="kwrd">void</span> onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_drawer);

   <span class="rem">// Retrieve the AppCompact Toolbar</span>
    Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
    setSupportActionBar(toolbar);

   <span class="rem">// Set the padding to match the Status Bar height</span>
    toolbar.setPadding(0, getStatusBarHeight(), 0, 0);
}</pre>

And finally we can see that the Status Bar is transparent and that our Toolbar has the right padding. Unfortunately the behavior between Lollipop and KitKat is totally different. In Lollipop the system draw a translucency of 20% while KitKat does not draw anything and the final result between the two systems is completely different:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Android-and-the-transparent-status-bar_1146A/image_thumb_6.png" width="418" height="166" />](http://blog.raffaeu.com/wp-content/uploads/Android-and-the-transparent-status-bar_1146A/image_6.png)

So, in order to get the final result looking the same on both systems we need to use a nice library called **Android System Bar Tint** available here on GitHub: [https://github.com/jgilfelt/SystemBarTint](https://github.com/jgilfelt/SystemBarTint "https://github.com/jgilfelt/SystemBarTint"). This library is capable of re-tinting the Status Bar with the color we need and we can also specify a level of transparency. So, because the default Material Design Status Bar should be 20% darker than the Toolbar color we can also say that the Status Bar should be 20% black, which correspond to the following RGB color: #20000000. (_But you can also provide a darker color and play with transparency, this is really up to you_).

So, going back to our onCreate method, after we setup the padding top for the Toolbar we can change the color of the Status Bar using the following code:

<pre class="csharpcode"><span class="rem">// create our manager instance after the content view is set</span>
SystemBarTintManager tintManager = <span class="kwrd">new</span> SystemBarTintManager(<span class="kwrd">this</span>);
<span class="rem">// enable status bar tint</span>
tintManager.setStatusBarTintEnabled(<span class="kwrd">true</span>);
<span class="rem">// enable navigation bar tint</span>
tintManager.setNavigationBarTintEnabled(<span class="kwrd">true</span>);
<span class="rem">// set the transparent color of the status bar, 20% darker</span>
tintManager.setTintColor(Color.parseColor(<span class="str">"#20000000"</span>));</pre>

At this point if we test again our application, the final result is pretty nice and also the overlap of the Navigation Drawer is exactly how is supposed to be in the Material Design Guidelines:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Android-and-the-transparent-status-bar_1146A/image_thumb_7.png" width="452" height="172" />](http://blog.raffaeu.com/wp-content/uploads/Android-and-the-transparent-status-bar_1146A/image_7.png)

The Next Video shows the final results running on KitKat and Lollipop device emulators using Genymotion.

<div id="scid:5737277B-5D6D-4f48-ABFC-DD9C333F4C5D:d0b86916-83e1-4508-8f19-53c9b4874db2" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px">
  <div>
  </div>
  
  <div style="width:448px;clear:both;font-size:.8em">
    The Final result on Lollipop and KitKat
  </div>
</div>