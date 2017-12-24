---
id: 88
title: Sharing assembly version in Visual Studio 2010.
date: 2011-12-11T17:58:05+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=88
permalink: /archive/2011/12/11/sharing-assembly-version-in-visual-studio-2010.aspx
categories:
  - 'C#'
  - NET World
  - TFS
---
Last week I came up with a fancy requirement that forced me to struggle a little bit in order to find an appropriate solution. Let’s say that we have a massive solution file, containing something like 100ish projects and we would like to keep the same assembly version number for all these projects.

In this article I will show you how the assembly version number works in .NET and what are the possible solutions, using Visual Studio.

### Assembly version in .NET

As soon as you add a new project (of any type) in Visual Studio 2010, you will come up with a default template that contains also a file _“AssemblyInfo.cs”_ if you are working with C# or _“AssemblyInfo.vb”_ if you are working with VB.NET.

[<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/4950d5cf-38ca-461b-8e36-67e72984cb26image_thumb.png" width="260" height="119" />](http://raffaeu.com/wp-content/uploads/2013/03/c67498fc-1ff4-4ae5-a87c-44380db74011image_2.png)

If we look at the content of this file we will discover that it contains a set of attributes used by MSBuild to prepare the assembly file (.dll or .EXE) with the information provided in this file. In order to change this information we have two options:

  1. Edit the AssemblyInfo.cs using the Visual Studio editor.   
    In this case we are interested in the following attributes, that we will need to change every time we want to increase the assembly version number:  
  
    <div class="csharpcode">
      <pre class="alt"><span class="lnum">   1:  </span><span class="kwrd">using</span> System.Reflection;</pre>
      
      <pre><span class="lnum">   2:  </span><span class="kwrd">using</span> System.Runtime.CompilerServices;</pre>
      
      <pre class="alt"><span class="lnum">   3:  </span><span class="kwrd">using</span> System.Runtime.InteropServices;</pre>
      
      <pre><span class="lnum">   4:  </span> </pre>
      
      <pre class="alt"><span class="lnum">   5:  </span>[assembly: AssemblyVersion(<span class="str">"1.0.0.0"</span>)]</pre>
      
      <pre><span class="lnum">   6:  </span>[assembly: AssemblyFileVersion(<span class="str">"1.0.0.0"</span>)]</pre></p>
    </div>

  2. Or, we can open the Project properties window from Visual Studio using the shortcut ALT+ENTER or by choosing “properties” of a VS project file from the Solution Explorer
      
      
    [<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/c0256692-f910-4a63-9b83-1656f930083aimage_thumb_1.png" width="260" height="182" />](http://raffaeu.com/wp-content/uploads/2013/03/f137c3a4-ac11-4b7b-afe9-4bab7fe508c5image_4.png) 

### How does the versioning work?

The first thing that I tried was to understand exactly how this magic number works in .NET.

If you go to the <a href="http://msdn.microsoft.com/en-us/library/51ket42z.aspx?ppud=4" target="_blank">online MSDN article</a>, you will find out that the version number of an assembly is composed by 4 numbers, and each one has a specific mean

**1.** **Major =** manually incremented for major releases, such as adding many new features to the solution. 

**0.** **Minor =** manually incremented for minor releases, such as introducing small changes to existing features. 

**0.** **Build =** typically incremented automatically as part of every build performed on the Build Server. This allows each build to be tracked and tested. 

**** **Revision =** incremented for QFEs (a.k.a. “hotfixes” or patches) to builds released into the Production environment (PROD). This is set to zero for the initial release of any major/minor version of the solution.

### Two different assembly version attributes, why?

I noticed that the _[assembly]_ attribute class exposes two different properties, Assembly Version and Assembly File Version. 

**AssemblyFileVersion**

<font color="#2e2e2e">This attribute should be incremented every time our build server (TFS) runs a build. Based on the previous description you should increase the third number, the build version number. This attribute should be placed in a different .cs file for each project to allow full control of it. </font>

**AssemblyVersion**

<font color="#2e2e2e">This attributes represents the version of the NET assembly you are referencing in your projects. If you increase this number in every TFS build, you will incur in the problem of changing your reference redirect every time the assembly version is increased.</font>

<font color="#2e2e2e">This number should be increased <strong>only</strong> when you release a new version of your assembly and it should be increase following the assembly versioning terminology (major, minor, …)</font>

### <font color="#2e2e2e">Control the Versioning in Visual Studio</font>

<font color="#2e2e2e">As I said before VS allows us to control the version number in different ways and in my opinion using the properties window is the easiest one. As soon as you change one of the version numbers from the properties window, also the AssembliInfo.cs file will be automatically changed.</font>

<font color="#2e2e2e">But what happens if we delete the version attributes from the assembly info file? </font>As expected VS will create an assembly with version 0.0.0.0 like the picture below:

[<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/9ae2db4f-d0ca-4147-a573-972ab31ad770image_thumb_2.png" width="260" height="230" />](http://raffaeu.com/wp-content/uploads/2013/03/f3305793-affa-44dc-9335-d8529e7954acimage_6.png)

<div style="border-bottom: #666666 1px solid; border-left: #666666 1px solid; padding-bottom: 3px; padding-left: 3px; padding-right: 3px; background: #efefef; border-top: #666666 1px solid; border-right: #666666 1px solid; padding-top: 3px">
  <p>
    <strong>Note:</strong> if we open the Visual Studio properties window for the project and we write down the version 1.0.0.1 for both, Assembly and AssemblyFile attribute, VS will re-create these two attributes in the AssemblyInfo.cs file.
  </p>
</div>

### Sharing a common Assembly version on multiple projects

Going back to the request I got, how can we setup a configuration in Visual Studio that allows us to share on multiple projects the same assembly version? A partial solution can be accomplished using shared linked files on Visual Studio.

Ok, what’s a shared linked file, first of all? A linked file is a file shortcut that points in multiple projects to the same single file instance. A detailed explanation of this mechanism is available on <a href="http://blogs.msdn.com/b/jjameson/archive/2009/04/02/linked-files-in-visual-studio-solutions.aspx" target="_blank">Jeremy Jameson’s blog at this page</a>.

Now, this is the solution I have created as an example where I share an AssemblyVersion.cs file and an AssemblyFileVersion.cs file to the entire Visual Studio solution.

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/0640c08e-6764-48d7-8ee0-beea94e7f6e7image_thumb_3.png" width="233" height="260" />](http://raffaeu.com/wp-content/uploads/2013/03/4b4a64ba-72e2-4ac0-ad51-dcb865381154image_8.png)

Using this approach we have one single place where we can edit the AssemblyFileVersion and the AssemblyVersion attributes. In order to accomplish this solution you need to perform the following steps:

  1. Delete the assembly version and the assembly file version attributes for all the existing AssemblyInfo.cs files
  2. Create in one project (the root project) a file called AssemblyFileVersion.cs containing **only** the attribute AssemblyFileVersion
  3. Create in one project (the root project) a file called AssemblyVersion.cs containing **only** the attribute AssenblyVersion
  4. Add as linked files these two files to all the existing projects
  5. Re-Build everything

### Final note on Visual Studio properties window

Even if my root project has now two files with the attributes AssemblyFileVersion and AssemblyVersion, when I open the Visual Studio properties window, it tries to search for these attributes in the AssemblyInfo.cs file, and clearly, it can’t find them anymore, so it does not display anything:

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/d24f2dce-5a08-410f-b32e-f1314db722b6image_thumb_4.png" width="260" height="74" />](http://raffaeu.com/wp-content/uploads/2013/03/c016850a-aaeb-4fa7-99ce-b578571c943aimage_10.png)

If you add a value to these textboxes Visual Studio will re-create the two attributes in the AssemblyInfo.cs file without taking care of the two new files we have created and as soon as you try to compile the project you will receive this nice error:

[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/3e98ef87-c024-46c4-ba5d-e882b1c9636fimage_thumb_5.png" width="260" height="93" />](http://raffaeu.com/wp-content/uploads/2013/03/528f6cf2-da4e-4b59-a36c-3948b34f81aaimage_12.png)

So, in order to use this solution you need to keep in mind that you **can’t edit the AssemblyFileVersion and the AssemblyVersion attributes from the VS properties window if they are not saved in the AssemblyInfo.cs file!**

<font color="#2e2e2e">I believe that MS should change this in the next versions of Visual Studio.</font>

<img style="border-bottom-style: none; border-left-style: none; border-top-style: none; border-right-style: none" class="wlEmoticon wlEmoticon-winkingsmile" alt="Winking smile" src="http://raffaeu.com/wp-content/uploads/2013/03/3e03e19c-bbf7-475a-a2ef-e4b770dca612wlEmoticon-winkingsmile_2.png" />