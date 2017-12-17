---
id: 742
title: Get started with Polymer 1.0 and WebStorm 11
date: 2016-02-17T15:57:40+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=742
permalink: /archive/:year/:month/:day/:title/
categories:
  - Android
  - Mobile
  - polymer
tags:
  - polymer
  - webstorm
---
So this year one of my goals is to learn <a href="https://www.polymer-project.org/1.0/" target="_blank">Polymer 1.0</a>. I followed this project for about a year now and I believe that the framework is mature enough to be adopted even into a production environment. Unfortunately Google didn’t make a great job regarding Polymer (in my own opinion) and there is not a great web development IDE available like they did with <a href="http://developer.android.com/sdk/index.html?gclid=CjwKEAiA0ZC2BRDpo_Pym8m-4n4SJAB5Bn4xNKjQhPsNg7cS11gzIfccXuL8unXUa3K56mYWZvYO_xoCvTfw_wcB" target="_blank">Android Studio</a> for Android development.

In the past I worked a lot with Visual Studio but also with JetBrains tools and I found <a href="https://www.jetbrains.com/webstorm/" target="_blank">WebStorm</a> a very reliable tool for web development, so I decided to start this article which will explain how to setup the correct development environment for Polymer 1.0 using WebStorm.

> **Note:**  
> I have both Linux and Windows 10 PCs, I don’t use MAC at all, and this article is all based on my Windows configuration. If you are going to use WebStorm and Polymer on Linux the tutorial is still valid, but if you are on MAC, well you are on your own <img class="wlEmoticon wlEmoticon-winkingsmile" style="border-top-style: none; border-left-style: none; border-bottom-style: none; border-right-style: none" alt="Winking smile" src="http://blog.raffaeu.com/wp-content/uploads/2016/02/wlEmoticon-winkingsmile.png" />

## Download the necessary tools

First of all, in order to have the proper environment setup correctly, especially in Windows, you MUST download all tools before you start to download WebStorm or Polymer, otherwise you will start the nightmare of “path not found” errors and similar.

  * **GIT**  
    GIT is a well-known command tool for managing GIT repositories. Even if you come from an SVN or TFS (Team Foundation Server) environment I would suggest you to get started on GIT because even companies like Microsoft are moving their source code into GitHub or BitBucket repositories and GIT is used by BOWER to download and synchronize components from Polymer.  
    [https://git-scm.com/](https://git-scm.com/ "https://git-scm.com/") 
      * **NODE.js**  
        Node is a JavaScript runtime build on Chrome V8 JavaScript engine. If you work with Polymer your primary use of Node is with the command NPM, which will be used in parallel with BOWER.  
        [https://nodejs.org/en/](https://nodejs.org/en/ "https://nodejs.org/en/") 
          * **BOWER**  
            If you come from Java you may be aware of “Maven Central” while if you come from .NET you may be aware of “Nuget”. Well BOWER is the same concept applied to web development. It allows you to download “packages” of CSS, JavaScript and HTML files packed as “components”. Without Node.js you can’t use BOWER because it requires the NPM (Node Package Manager) command tool.  
            [http://bower.io/](http://bower.io/ "http://bower.io/")</ul> 
        Now, after you install GIT, open your command prompt and type:
        
        <pre>git --version
<strong><font color="#0000ff"># and the output will be</font></strong>
git version 2.6.2.windows.1</pre>
        
        If you don&#8217;t get so far it means that you don&#8217;t have the GIT bin folder registered as a Windows Environment Variable. The second step is to verify that node is installed and that’s the same of GIT, just type 
        
        <pre>npm version
<strong><font color="#0000ff"># and the output will be</font></strong>
{ npm: '3.6.0',
  ares: '1.10.1-DEV',
  http_parser: '2.6.1',
  icu: '56.1',
  modules: '47',
  node: '5.6.0',
  openssl: '1.0.2f',
  uv: '1.8.0',
  v8: '4.6.85.31',
  zlib: '1.2.8' }</pre>
        
        Great. Last step is to install the package BOWER from Node. 
        
        <pre>npm install -g bower</pre>
        
        And then verify that bower is installed by typing:
        
        <pre>bower --version</pre>
        
        So now you are 100% sure that you can move forward and prepare your dev environment. 
        
        ## Install and Configure WebStorm
        
        At the time I am writing this article WebStorm is available in the version 11. I assume that in the future nothing will change regarding this part of the setup, especially because all JetBrains IDE are based on the same core IntelliJ IDE. WebStorm can be downloaded here: [https://www.jetbrains.com/webstorm/](https://www.jetbrains.com/webstorm/ "https://www.jetbrains.com/webstorm/") and you can use it for 30 days, then you need to buy a license or apply for a free license.
        
        After WebStorm is installed, open the IDE and choose “new empty project” and name it polymer_test like I did:
        
        [<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2016/02/image_thumb.png" width="520" height="120" />](http://blog.raffaeu.com/wp-content/uploads/2016/02/image.png)
        
        Now you should have an empty project and WebStorm ready to rock. If you look at the bottom of the IDE you will see a “Terminal” window. This is an instance of your DOS command prompt (in Windows) and your Terminal prompt (in Linux). Just double check that everything is fine by typing something like “bower –version” or “git –version”:
        
        [<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2016/02/image_thumb-1.png" width="520" height="237" />](http://blog.raffaeu.com/wp-content/uploads/2016/02/image-1.png)
        
        ### Step 01 – Init Bower
        
        This is the basic of polymer which is needed in order to get started, you need to run the command “bower init” which will prepare a JSON file with the project configuration information. _If you don’t know what to do here, just keep press ENTER until the json file is created_.
        
        [<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2016/02/image_thumb-2.png" width="520" height="264" />](http://blog.raffaeu.com/wp-content/uploads/2016/02/image-2.png)
        
        Now you should have in your root a file named _bower.json_ which contains your project’s configuration information.
        
        ### Step 02 – Download the core of Polymer
        
        Second step is to download the Polymer core. This is required by any Polymer project. type “bower install –save Polymer/polymer#^1.2.0” in order to download the latest stable version of Polymer which is the 1.2.0 version.   
        _If you don’t specify the version, bower will download all available version and then ask you which version you want to use_.
        
        [<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2016/02/image_thumb-3.png" width="520" height="120" />](http://blog.raffaeu.com/wp-content/uploads/2016/02/image-3.png)
        
        At this point your project will have a new folder called “bower_components” and the Polymer core component’s folders:
        
        [<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2016/02/image_thumb-4.png" width="320" height="120" />](http://blog.raffaeu.com/wp-content/uploads/2016/02/image-4.png)
        
        Final, create a new page and name it “index.html” with the following HTML code:
        
        <pre>&lt;!DOCTYPE html&gt;<br />&lt;html lang="en"&gt;<br />&lt;head&gt;<br />    &lt;meta charset="UTF-8"&gt;<br />    &lt;title&gt;My First Polymer Application&lt;/title&gt;<br /><strong>    &lt;script type="text/javascript" src="bower_components/webcomponentsjs/webcomponents.js"&gt;&lt;/script&gt;<br />    &lt;link href="bower_components/polymer/polymer.html" rel="import"&gt;<br /></strong>&lt;/head&gt;<br />&lt;body&gt;<br /><br />&lt;/body&gt;<br />&lt;/html&gt;</pre>
        
        Now your page is ready to be used as a Polymer page, you only need to import the components that you need or create your own ones. If you want to be sure that everything works as expected, just right click on your .html file and choose “browse with” and test it on your preferred browser (I assume Chrome):
        
        [<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2016/02/image_thumb-5.png" width="370" height="198" />](http://blog.raffaeu.com/wp-content/uploads/2016/02/image-5.png)
        
        ### Step 03 – Download the Material Design components
        
        If you know already that you are going to work with Material Design components, then you can easily download the whole iron-components and paper-components into your project with two simple bower commands:
        
        <pre>bower install --save PolymerElements/iron-elements
bower install --save PolymerElements/paper-elements</pre>
        
        Then you can try to create your first page by importing, for example, the Material Design Toolbar components as following:
        
        <pre>&lt;!DOCTYPE html&gt;<br />&lt;html lang="en"&gt;<br />&lt;head&gt;<br />    &lt;meta charset="UTF-8"&gt;<br />    &lt;title&gt;My First Polymer Application&lt;/title&gt;<br />    &lt;script type="text/javascript" src="bower_components/webcomponentsjs/webcomponents.js"&gt;&lt;/script&gt;<br />    &lt;link href="bower_components/polymer/polymer.html" rel="import"&gt;<br /><br /><strong><font color="#0000ff">    &lt;!-- Toolbar element --&gt;<br />    &lt;link href="bower_components/paper-toolbar/paper-toolbar.html" rel="import"&gt;<br /></font></strong>&lt;/head&gt;<br />&lt;body&gt;<br /><strong><font color="#0000ff">    &lt;paper-toolbar&gt;<br />        &lt;div class="title"&gt;My Toolbar&lt;/div&gt;<br />    &lt;/paper-toolbar&gt;<br /></font></strong>&lt;/body&gt;<br />&lt;/html&gt;</pre>
        
        And you should have a nice page with a basic Material Design Toolbar like mine:
        
        [<img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2016/02/image_thumb-6.png" width="420" height="87" />](http://blog.raffaeu.com/wp-content/uploads/2016/02/image-6.png)
        
        Enjoy!