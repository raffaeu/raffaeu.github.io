---
id: 158
title: 'VSDocMan review&ndash;Auto generate VS Help'
date: 2011-01-30T17:47:57+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=158
permalink: /archive/2011/01/30/vsdocman-reviewauto-generate-vs-help.aspx
categories:
  - NET World
  - Technical Documents
---
In these days I have been busy in the office by implementing a mechanism of C.I. (Continuous Integration) by releasing some facilities assemblies so that the developers that are working with me will be able to work always with the latest version of a specific .dll.

One of the most important thing I noticed when you work with a third-party or a legacy .dll is the **documentation** needed within the code. If you think for a moment on what you do when you work with .NET Framework, every time you do not know how to use a specific component or a specific class, you simply type [www.msdn.com](http://www.msdn.com) and you start to search in the help directory for some sample code or for a documentation.

The same important thing has to be done when you work with a third-party component or simply when **<u>you are in charge of releasing a facility assembly/framework</u>** like I do in my office.

Before showing you what I have found over the internet to solve the problem of generating the documentation, I want to explain you a little bit how the Visual Studio documentation system works.

### Visual Studio documentation mechanism

One of the features included in Visual Studio, since one of the first version (maybe 2003/2005?) is the XML embedded documentation that allows you to write documentation in an XML format like the following example:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:f06d4beb-d792-4130-9490-20d9d4acc4e8" class="wlWriterEditableSmartContent">
  <div class="le-pavsc-container">
    <div class="le-pavsc-titleblock">
      Sample documented Class
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px; white-space: nowrap">
        <li>
          <span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"><summary></span>
        </li>
        <li class="even">
          <span style="color:#808080">///</span><span style="color:#008000"> Class level summary documentation goes here.</span><span style="color:#808080"></summary></span>
        </li>
        <li>
          <span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"><remarks></span>
        </li>
        <li class="even">
          <span style="color:#808080">///</span><span style="color:#008000"> Longer comments can be associated with a type or member </span>
        </li>
        <li>
          <span style="color:#808080">///</span><span style="color:#008000"> through the remarks tag</span><span style="color:#808080"></remarks></span>
        </li>
        <li class="even">
          <span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">class</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">TestClass</span>
        </li>
        <li>
          <span style="color:#ffffff">{</span>
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"><summary></span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> Store for the name property</span><span style="color:#808080"></summary></span>
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#cc7832">private</span><span style="color:#ffffff"> </span><span style="color:#cc7832">string</span><span style="color:#ffffff"> _name = </span><span style="color:#cc7832">null</span><span style="color:#ffffff">;</span>
        </li>
        <li>
           
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"><summary></span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> The class constructor. </span><span style="color:#808080"></summary></span>
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> TestClass()</span>
        </li>
        <li>
              <span style="color:#ffffff">{</span>
        </li>
        <li class="even">
                  <span style="color:#ffffff"></span><span style="color:#808080">// </span><span style="color:#00008b">TODO: Add Constructor Logic here</span>
        </li>
        <li>
              <span style="color:#ffffff">}</span>
        </li>
        <li class="even">
           
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"><summary></span>
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> Name property </span><span style="color:#808080"></summary></span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"><value></span>
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> A value tag is used to describe the property value</span><span style="color:#808080"></value></span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">string</span><span style="color:#ffffff"> Name</span>
        </li>
        <li class="even">
              <span style="color:#ffffff">{</span>
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#cc7832">get</span>
        </li>
        <li class="even">
                  <span style="color:#ffffff">{</span>
        </li>
        <li>
                      <span style="color:#ffffff"></span><span style="color:#cc7832">if</span><span style="color:#ffffff"> (_name == </span><span style="color:#cc7832">null</span><span style="color:#ffffff">)</span>
        </li>
        <li class="even">
                      <span style="color:#ffffff">{</span>
        </li>
        <li>
                          <span style="color:#ffffff"></span><span style="color:#cc7832">throw</span><span style="color:#ffffff"> </span><span style="color:#cc7832">new</span><span style="color:#ffffff"> System.</span><span style="color:#ffc66d">Exception</span><span style="color:#ffffff">(</span><span style="color:#a5c25c">&#8220;Name is null&#8221;</span><span style="color:#ffffff">);</span>
        </li>
        <li class="even">
                      <span style="color:#ffffff">}</span>
        </li>
        <li>
                      <span style="color:#ffffff"></span><span style="color:#cc7832">return</span><span style="color:#ffffff"> _name;</span>
        </li>
        <li class="even">
                  <span style="color:#ffffff">}</span>
        </li>
        <li>
              <span style="color:#ffffff">}</span>
        </li>
        <li class="even">
           
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"><summary></span>
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> Description for SomeMethod.</span><span style="color:#808080"></summary></span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"><param name=&#8221;s&#8221;></span><span style="color:#008000"> Parameter description for s goes here</span><span style="color:#808080"></param></span>
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"><seealso cref=&#8221;System.String&#8221;></span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> You can use the cref attribute on any tag to reference a type or member </span>
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> and the compiler will check that the reference exists. </span><span style="color:#808080"></seealso></span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> SomeMethod(</span><span style="color:#cc7832">string</span><span style="color:#ffffff"> s)</span>
        </li>
        <li class="even">
              <span style="color:#ffffff">{</span>
        </li>
        <li>
              <span style="color:#ffffff">}</span>
        </li>
        <li class="even">
           
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"><summary></span>
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> Some other method. </span><span style="color:#808080"></summary></span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"><returns></span>
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> Return results are described through the returns tag.</span><span style="color:#808080"></returns></span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"><seealso cref=&#8221;SomeMethod(string)&#8221;></span>
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> Notice the use of the cref attribute to reference a specific method </span><span style="color:#808080"></seealso></span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">int</span><span style="color:#ffffff"> SomeOtherMethod()</span>
        </li>
        <li class="even">
              <span style="color:#ffffff">{</span>
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#cc7832">return</span><span style="color:#ffffff"> </span><span style="color:#6897bb"></span><span style="color:#ffffff">;</span>
        </li>
        <li class="even">
              <span style="color:#ffffff">}</span>
        </li>
        <li>
           
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"><summary></span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> The entry point for the application.</span>
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"></summary></span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> </span><span style="color:#808080"><param name=&#8221;args&#8221;></span><span style="color:#008000"> A list of command line arguments</span><span style="color:#808080"></param></span>
        </li>
        <li class="even">
              <span style="color:#ffffff"></span><span style="color:#cc7832">static</span><span style="color:#ffffff"> </span><span style="color:#cc7832">int</span><span style="color:#ffffff"> Main(System.</span><span style="color:#ffc66d">String</span><span style="color:#ffffff">[] args)</span>
        </li>
        <li>
              <span style="color:#ffffff">{</span>
        </li>
        <li class="even">
                  <span style="color:#ffffff"></span><span style="color:#808080">// </span><span style="color:#00008b">TODO: Add code to start application here</span>
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#cc7832">return</span><span style="color:#ffffff"> </span><span style="color:#6897bb"></span><span style="color:#ffffff">;</span>
        </li>
        <li class="even">
              <span style="color:#ffffff">}</span>
        </li>
        <li>
          <span style="color:#ffffff">}</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

By using this documentation technique you are giving to your code an additional immeasurable value; especially if you work with some Visual Studio plugin like <a href="http://www.jetbrains.com/resharper/" target="_blank">Resharper</a>, for example, you will come up with a nice and well formatted intellisense suggestion if you decorate the code in this way.

<a href="http://raffaeu.com/wp-content/uploads/2013/03/23a88a6b-c700-42c0-aed2-71fd8daaea8eparameter_info_csharp_2.png" rel="lightbox"><img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="parameter_info_csharp" border="0" alt="parameter_info_csharp" src="http://raffaeu.com/wp-content/uploads/2013/03/0793903f-891f-4042-b3f7-5d9f0027538cparameter_info_csharp_thumb.png" width="479" height="164" /></a>

If you want to learn more about it, MSDN offers a great section about the XML documentation in Visual Studio 2010 and how it should be used.

[http://msdn.microsoft.com/en-us/library/b2s063f7.aspx](http://msdn.microsoft.com/en-us/library/b2s063f7.aspx "http://msdn.microsoft.com/en-us/library/b2s063f7.aspx")

This is a great article that gives you a full overview of this technology: [http://msdn.microsoft.com/en-us/magazine/cc302121.aspx](http://msdn.microsoft.com/en-us/magazine/cc302121.aspx "http://msdn.microsoft.com/en-us/magazine/cc302121.aspx")

### Documentation with VSDocMan

Last week, after I started working on this C.I. project I just realized that now, more than everything, the other guys that will use my code will need tons of documentation in order to understand how to use my toolkits an facilities. Well I do not want to “blown my own trumpet” but I can easily say that I usually document whatever I write and that I use a lot <a href="http://www.jetbrains.com/resharper" target="_blank">Resharper</a> and <a href="http://code.msdn.microsoft.com/sourceanalysis" target="_blank">StyleCop</a> to make my code readable and uniform. Unfortunately I am very allergic to .chm files in general.

I have this tool that is called <a href="http://www.helixoft.com/vsdocman/overview.html" target="_blank">VsDocMan</a> my lifeline and I want to show you how it works; trust me it is simply AWESOME! <img style="border-bottom-style: none; border-right-style: none; border-top-style: none; border-left-style: none" class="wlEmoticon wlEmoticon-hotsmile" alt="Hot smile" src="http://raffaeu.com/wp-content/uploads/2013/03/46dc0bd6-d0ee-4c31-930c-7211de631072wlEmoticon-hotsmile_2.png" />

First of all you can generate VS documentation with a simple Click on the toolbar or in the context menu; in fact VSDocMan allows you to self-document your classes with some pre-defined XML schemas

![Automatic insertion of XML comments](http://www.helixoft.com/images/stories/vsdocman/crop_addVBcomment.png)

Another interesting feature is the **Comment Editor** command, with this command VSDocMan will show you an interactive MSDN style documentation of how the final doc will look like so that you can easily change your documentation structure in a WYSIWYG editor without the need of struggling with the ugly XML

![Editor for XML documentation comments](http://www.helixoft.com/images/stories/vsdocman/crop_editorVBcomment.png)

Plus it allows you to generate a dynamic class diagram that is clickable and that points to the corresponding documentation file!

VSDOCMan generates a lot of different documentations: .chm, MSDN style, MSDN V2, HTML, a full MSDN HTML web site and much more. It is very cheap in my opinion for what it does and if you are looking for a **<u>professional and fully working solution</u>** to generate documentation from your code, you should seriously consider to spend some time and money on this AWESOME product.

As usual, my two cents. <img style="border-bottom-style: none; border-right-style: none; border-top-style: none; border-left-style: none" class="wlEmoticon wlEmoticon-justkidding" alt="Just kidding" src="http://raffaeu.com/wp-content/uploads/2013/03/16aef921-24be-4942-bce9-f761e3965e43wlEmoticon-justkidding_2.png" />