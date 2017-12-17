---
id: 689
title: Android Plugin Application
date: 2015-05-31T17:23:00+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=689
permalink: /archive/:year/:month/:day/:title/
categories:
  - Android
tags:
  - android
  - plugins
---
For one of the project I am working at the moment I have the need to implement a plug-in architecture where the main application is just a “view holder” and all the behaviors of the application are provided by a set of plug-ins.

The advantage of this approach is that in the future I will not need to re-distribute my entire framework over the marketplace but simply release new plug-ins that the customer can add or update the existing one.

> **Note:** the code presented in this article is not optimized and its only purpose is to explain one of the possible solutions to implement a custom plug-in architecture in Android. The methods exposed are not optimized and do not use an asynchronous pattern so I suggest to refactor them before adopting this code into your real applications

## What is a plug-in architecture?

Before deep diving into the code I want to take a moment and explain what I personally mean for plug-in architecture, using the following diagram:

![](http://jpf.sourceforge.net/resources/images/jpf-diagram.png)

The previous picture represents a description of the JPF, the Java Plugin Framework, which is a similar solution to the one we are going to implement. The architecture is primarily composed by two different components. 

One component is the main application, the agnostic framework capable to load plug-ins. The second component is the plug-in registry, a repository that inform the system about the available plug-ins installed into the system.

In Android we have tons of different ways to implement a plug-in architecture. For example we can have a plug-in composed by a .JAR package that contains activities and code related to the plug-in. But I found out that in Android the best way to package things is to use the .apk system. With an .apk I can include Activities, Fragments, resources and layouts like a standalone application with the advantage of using some sort of _contracts_ to force the code to be in a certain way.

## Retrieve available packages

The basic project is a simple Android Application with a main activity. The main activity contains a ListView that will display all the available .apk that we can consider a plug-in for our application.

But first of all, let’s see how we can retrieve a list of installed .apk using some basic Android APIs. First of all I need to create a custom ListView item that can be used to display the information relative to a package. And this is the custom class:

<pre class="csharpcode">package ltd.netarchitectures.na_plugins;

import android.graphics.drawable.Drawable;

<span class="kwrd">public</span> <span class="kwrd">class</span> ApplicationDetail {

    <span class="kwrd">private</span> CharSequence label;
    <span class="kwrd">private</span> CharSequence name;
    <span class="kwrd">private</span> Drawable icon;

    <span class="kwrd">public</span> ApplicationDetail(CharSequence label, CharSequence name, Drawable icon) {
        <span class="kwrd">this</span>.label = label;
        <span class="kwrd">this</span>.name = name;
        <span class="kwrd">this</span>.icon = icon;
    }

    <span class="kwrd">public</span> CharSequence getLabel() {
        <span class="kwrd">return</span> label;
    }

    <span class="kwrd">public</span> CharSequence getName() {
        <span class="kwrd">return</span> name;
    }

    <span class="kwrd">public</span> Drawable getIcon() {
        <span class="kwrd">return</span> icon;
    }
}</pre>

So with this custom class we can represents an available package. For now the information exposed are enough but we can retrieve more information like the company who made the package, the size of the package, the version and so on. We can also expose the package to see how many Fragments or Activities are available. Again, here the limit is your imagination.

Now we need to fetch all available packages. Because we do not have any plug-in available yet, let’s see how we can fetch all the available **applications** just to start to have a look at the android API.

<pre class="csharpcode"><span class="kwrd">private</span> <span class="kwrd">void</span> loadApplication(){
        <span class="rem">// package manager is used to retrieve the system's packages </span>
        packageManager = getPackageManager();
        applications = <span class="kwrd">new</span> ArrayList&lt;ApplicationDetail&gt;();
        <span class="rem">// we need an intent that will be used to load the packages</span>
        Intent intent = <span class="kwrd">new</span> Intent(Intent.ACTION_MAIN, <span class="kwrd">null</span>);
        <span class="rem">// in this case we want to load all packages available in the launcher</span>
        intent.addCategory(Intent.CATEGORY_LAUNCHER);
        List&lt;ResolveInfo&gt; availableActivities = packageManager.queryIntentActivities(intent,0);
        <span class="rem">// for each one we create a custom list view item</span>
        <span class="kwrd">for</span>(ResolveInfo resolveInfo:availableActivities){
            ApplicationDetail applicationDetail = <span class="kwrd">new</span> ApplicationDetail(
                    resolveInfo.loadLabel(packageManager),
                    resolveInfo.activityInfo.packageName,
                    resolveInfo.activityInfo.loadIcon(packageManager));
            applications.add(applicationDetail);
        }
    }</pre>

At the end (_I skip the code to create a custom list view item cause it should be quite easy to implement, anyway you can find it in the source code of this article_) we will have a list view populated with all available applications. In my case in alphabetical order:

[<img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; border-left: 0px; display: block; padding-right: 0px; margin-right: auto" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Android-Plugin-Application_701E/image_thumb.png" width="205" height="310" />](http://blog.raffaeu.com/wp-content/uploads/Android-Plugin-Application_701E/image.png)

## Create a plug-in

In order to not loose any of the advantages of the android application package we want to distribute our plug-ins as standalone packages **but** we don’t want that the user is capable to execute the packages as stand alone packages. 

First of all, I want to make a little explanation of how android works and how each package is treated by the underlying Linux system:

  * The Android operating system is a multi-user Linux system in which <font style="background-color: #ffff00">each app is a different user</font>. 
  * By default, the system assigns each app a unique Linux user ID (the ID is used only by the system and is unknown to the app). The system sets permissions for all the files in an app so that only the user ID assigned to that app can access them. 
  * <font style="background-color: #ffff00">Each process has its own virtual machine</font> (VM), so an app&#8217;s code runs in isolation from other apps. 
  * By default, <font style="background-color: #ffff00">every app runs in its own Linux process</font>. Android starts the process when any of the app&#8217;s components need to be executed, then shuts down the process when it&#8217;s no longer needed or when the system must recover memory for other apps.

All right, so first of all we need to create a new Android project and set the project to run in background. The project will have its own icon and activities and bla bla but it cannot be shown into the home launcher.

<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">activity</span>
            <span class="attr">android:name</span><span class="kwrd">=".MainActivity"</span>
            <span class="attr">android:label</span><span class="kwrd">="@string/app_name"</span> <span class="kwrd">&gt;</span>
            <span class="kwrd">&lt;</span><span class="html">intent-filter</span><span class="kwrd">&gt;</span>
                <span class="kwrd">&lt;</span><span class="html">action</span> <span class="attr">android:name</span><span class="kwrd">="android.intent.action.MAIN"</span> <span class="kwrd">/&gt;</span>
                <span class="rem">&lt;!--</span>
<span class="rem">                &lt;category android:name="android.intent.category.LAUNCHER" /&gt;</span>
<span class="rem">                --&gt;</span>
            <span class="kwrd">&lt;/</span><span class="html">intent-filter</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;/</span><span class="html">activity</span><span class="kwrd">&gt;</span></pre>

And then the application is installed into our system, but is not visible in the launcher like the following screenshot:

[<img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; border-left: 0px; display: block; padding-right: 0px; margin-right: auto" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Android-Plugin-Application_701E/image_thumb_3.png" width="260" height="206" />](http://blog.raffaeu.com/wp-content/uploads/Android-Plugin-Application_701E/image_3.png)

The next step is to **share** a custom INTENT between my plug-in application and my main application and set the category of the plug-in to this custom intent. In this way my list view will be populated **only** with the list of available plugins.

If you pay attention to the Android manifest the INTENT is nothing more than a custom string, so in my plugin manifest I will change the intent in the following one:

<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">activity</span>
    <span class="attr">android:name</span><span class="kwrd">=".MainActivity"</span>
    <span class="attr">android:label</span><span class="kwrd">="@string/app_name"</span> <span class="kwrd">&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">intent-filter</span><span class="kwrd">&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">action</span> <span class="attr">android:name</span><span class="kwrd">="android.intent.action.MAIN"</span> <span class="kwrd">/&gt;</span>
        <span class="kwrd">&lt;</span><span class="html">category</span> <span class="attr">android:name</span><span class="kwrd">="ltd.netarchitectures.PLUGIN"</span> <span class="kwrd">/&gt;</span>
    <span class="kwrd">&lt;/</span><span class="html">intent-filter</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">activity</span><span class="kwrd">&gt;</span></pre>

And inside my ListView adapter I will load the activities that **only** implement my INTENT like this one:

<pre class="csharpcode">packageManager = getPackageManager();
applications = <span class="kwrd">new</span> ArrayList&lt;&gt;();
Intent intent = <span class="kwrd">new</span> Intent(Intent.ACTION_MAIN, <span class="kwrd">null</span>);
intent.addCategory(<span class="str">"ltd.netarchitectures.PLUGIN"</span>);</pre>

And now when I start my main application **only** my plugins will be loaded:

[<img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; float: none; padding-top: 0px; padding-left: 0px; margin-left: auto; border-left: 0px; display: block; padding-right: 0px; margin-right: auto" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Android-Plugin-Application_701E/image_thumb_4.png" width="260" height="126" />](http://blog.raffaeu.com/wp-content/uploads/Android-Plugin-Application_701E/image_4.png)

Easy, isn’t it? In the next article I will explain how we can execute this **plugin** within the same thread (Linux VM) of the main activity and how we can control when a new plug-in is installed into the system.