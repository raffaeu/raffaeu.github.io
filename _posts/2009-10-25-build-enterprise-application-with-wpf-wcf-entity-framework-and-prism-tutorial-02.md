---
id: 242
title: Build enterprise application with WPF, WCF, Entity Framework and Prism. Tutorial 02.
date: 2009-10-25T11:05:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=242
permalink: /archive/2009/10/25/build-enterprise-application-with-wpf-wcf-entity-framework-and-prism-tutorial-02.aspx
categories:
  - 'C#'
  - Design Pattern
  - WCF
  - WPF
---
<blockquote style="border-bottom: black 1px solid; border-left: black 1px solid; padding-bottom: 5px; background-color: #ffff00; margin: 5px; padding-left: 5px; padding-right: 5px; border-top: black 1px solid; border-right: black 1px solid; padding-top: 5px">
  <p>
    <strong style="color: #ff0000">Update: </strong>source code and documentation are now available on <a href="http://www.codeplex.com" target="_blank">CodePlex</a> at this address: <a title="http://prismtutorial.codeplex.com/" href="http://prismtutorial.codeplex.com/">http://prismtutorial.codeplex.com/</a>
  </p>
</blockquote>

## Build a generic repository with the Entity Framework.

In the previous series of articles about M-V-VM we used the AdventureWorks database, available on CodePlex.   
After this article (<a href="http://blog.raffaeu.com/archive/2009/06/05/wpf-and-vmmv-tutorial-02-the-model.aspx" target="_blank">configure a data layer with LinQ-to-SQL</a>) I have got some readerâ€™s emails asking me this and that. So, I am going to give you all those answers right now.

### Download and configure the Adventure Works database.

On my dev PC I have SQL Server 2008 Standard version, but this tutorial is working also with the express or any other version.

  1. Go to <a href="http://msftdbprodsamples.codeplex.com/" target="_blank">CodePlex Adventure Works</a> web site project and download the AW available version (light and full). I have downloaded the package called **_â€Adventure Works All Databasesâ€._** 
  2. Go into the folder tree **_Tools/Samples_** and you will find the .mdf and .ldf and also the T-SQL script if you want to run it from a prompt command line. 
  3. Open your SSMS and from the **_master_** database, install the version that you prefer. My samples work with both version, as I use always the Adventure Works LT (Light version). 

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_9E03/image.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_9E03/image_thumb.png" width="260" height="185" /></a> 

  1. <font color="#ff0000"><strong><em>Mandatory: </em></strong></font><font color="#000000">as I use Windows Authentication in all my samples, you must fix that in your local Db and change the connection string in my samples.</font> 

### Open Visual Studio and create the folder three.

As we are going to work with an enterprise application, in order to simulate the 3 tiers, I have created 3 _**solution folders**,_ one for the data service, one for the client part and one for the test. We will expand these folders during the building process.

The initial solution three will be:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_9E03/image_3.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_9E03/image_thumb_3.png" width="260" height="190" /></a>**_Solution name:_** PrismTutorial   
**_DataLayer:_** PrismTutorial.DataLayer   
**_Â  TDD:_** Test.DataLayer   
**_Consuming Service:_** PrismTutorial.WCF 

### Working with Entity Framework.

In the first series of tutorials, we used LinQ-to-SQL because:

  * Itâ€™s easier, Itâ€™s faster, It uses the relation 1 to 1 with the database tables and so on. 

For this tutorial we are going to use the Entity Framework because this one is the real OR/M coming from Microsoft so in an enterprise application itâ€™s better to spend more time and effort on building something stronger and reusable.

Open you VS data layer solution, right click and add an new component, **_ADO.NET Entity Data Model_** and configure it to reflect the Adventure Works LT database installed into your machine.

After that VS will show up a **_entity model designer_** window very similar to the one that we used with Linq-to-SQL.

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_9E03/image_4.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_9E03/image_thumb_4.png" width="260" height="142" /></a>If you want to be compliant with my settings, this is how I have configured my Entity Framework:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_9E03/image_5.png" rel="lightbox[tutorial]"><font color="#333333"></font><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_9E03/image_thumb_5.png" width="260" height="193" /></a>

### Itâ€™s time to play, the Entity Framework repository wrap!

Like any software architect, I try to fit always everything in a more generic, reusable and more readable pattern. If we want to have a reusable **_UnitOfWork_** associated with a **_Repository_** engine, we must wrap the Entity Framework inside something more generics. Here comes the pain â€¦.

The final result I would like to have is this one:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_9E03/image_6.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_9E03/image_thumb_6.png" width="236" height="260" /></a> 

#### IRepository interface.

First of all we have to define a generic contract (interface) that we will use as our repository. As we want to be **_generic_** we need something like this:

<pre style="border-bottom: #004080 1px solid; border-left: #004080 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #004080 1px solid; border-right: #004080 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  1: <span style="color: #0000ff">using</span> System;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  2: <span style="color: #0000ff">using</span> System.Collections.Generic;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  3: <span style="color: #0000ff">using</span> System.Linq.Expressions;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  4: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  5: <span style="color: #0000ff">namespace</span> PrismTutorial.DataLayer {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  6:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">interface</span> IRepository:IDisposable {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  7:         <span style="color: #008000">//Add a new Entity</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  8:         <span style="color: #0000ff">int</span> Add&lt;T&gt;(T entity);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  9:         <span style="color: #008000">//Count the number of entities available</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 10:         <span style="color: #0000ff">long</span> Count&lt;T&gt;();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 11:         <span style="color: #008000">//Count using a filer</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 12:         <span style="color: #0000ff">long</span> Count&lt;T&gt;(Expression&lt;Func&lt;T, <span style="color: #0000ff">bool</span>&gt;&gt; expression);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 13:         <span style="color: #008000">//Delete an existing entity</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 14:         <span style="color: #0000ff">int</span> Delete&lt;T&gt;(T entity);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 15:         <span style="color: #008000">//List all the available entities</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 16:         IList&lt;T&gt; GetAll&lt;T&gt;();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 17:         <span style="color: #008000">//List the entities using a filter</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 18:         IList&lt;T&gt; GetAll&lt;T&gt;(Expression&lt;Func&lt;T, <span style="color: #0000ff">bool</span>&gt;&gt; expression);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 19:         <span style="color: #008000">//Get a single entity</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 20:         T GetSingle&lt;T&gt;(Expression&lt;Func&lt;T,<span style="color: #0000ff">bool</span>&gt;&gt; expression);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 21:         <span style="color: #008000">//Update an existing entity</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 22:         <span style="color: #0000ff">int</span> Update&lt;T&gt;(T entity);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 23:     }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 24: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 25: </pre>


<p>
  At this point we have a generic implementation of the repository pattern and we do not need to worry about what we are going to save, update or delete.
</p>


<h4>
  BaseRepository to handle the dispose of the data context.
</h4>


<p>
  The second problem that we have to fix is more related to how the entity framework uses the <strong><em>context</em></strong> in a web application. I have found this article over the web: <a title="http://blog.zoolutions.se/post/2009/03/26/Generic-Repository-for-Entity-Framework.aspx" href="http://blog.zoolutions.se/post/2009/03/26/Generic-Repository-for-Entity-Framework.aspx">http://blog.zoolutions.se/post/2009/03/26/Generic-Repository-for-Entity-Framework.aspx</a>, really interesting, so I am going to use the same type of solution.
</p>


<p>
  We need a simple base class that is going to handle the data context and close it if not needed. We can also use the <strong><em>using clause</em></strong> in each method of our repository. I want to use this solution just because itâ€™s more clean and doesnâ€™t force me to write too much code inside the final repository.
</p>


<pre style="border-bottom: #004080 1px solid; border-left: #004080 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #004080 1px solid; border-right: #004080 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  1: <span style="color: #0000ff">using</span> System;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  2: <span style="color: #0000ff">using</span> System.Collections.Generic;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  3: <span style="color: #0000ff">using</span> System.Linq;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  4: <span style="color: #0000ff">using</span> System.Text;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  5: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  6: <span style="color: #0000ff">namespace</span> PrismTutorial.DataLayer {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  7:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">abstract</span> <span style="color: #0000ff">class</span> BaseRepository {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  8:         <span style="color: #008000">//Our entity framework engine used in the solution</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  9:         <span style="color: #0000ff">internal</span> ADVConnection _context;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 10:         <span style="color: #008000">//Switch that tells us if the datacontext is reused</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 11:         <span style="color: #0000ff">internal</span> <span style="color: #0000ff">bool</span> _contextReused;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 12:         
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 13:         <span style="color: #008000">//This return the current, or a new connection through the EF</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 14:         <span style="color: #0000ff">public</span> ADVConnection GetObjectContext() {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 15:             <span style="color: #0000ff">if</span> (!_contextReused) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 16:                 <span style="color: #0000ff">return</span> <span style="color: #0000ff">new</span> ADVConnection();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 17:             }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 18:             <span style="color: #0000ff">return</span> _context;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 19:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 20: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 21:         <span style="color: #008000">//This is the public method that we will call from our repository</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 22:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> ReleaseObjectContextIfNotReused() {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 23:             <span style="color: #0000ff">if</span> (!_contextReused) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 24:                 ReleaseObjectContext();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 25:             }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 26:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 27: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 28:         <span style="color: #008000">//Simple dispose of the current EF</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 29:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> ReleaseObjectContext() {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 30:             <span style="color: #0000ff">if</span> (_context != <span style="color: #0000ff">null</span>) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 31:                 _context.Dispose();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 32:             }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 33:             _contextReused = <span style="color: #0000ff">false</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 34:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 35:     }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 36: }</pre>


<p>
  Cool, we just need now to wrap all together and play with the data.
</p>


<h4>
  The concrete repository.
</h4>


<p>
  For the repository pattern there are two implementations, one is to keep everything generic and have 1 repository for the entire domain. The second one is to have a repository for each entity, like UserRepository, ProductRepository, and implement the CRUD operations related to the entity, like AddUser, AddProduct and so on.
</p>


<p>
  I would suggest the second solution only if you need to include some <strong><em>business logic</em></strong> in your CRUD operations or there are some special development requirements. So letâ€™s go for the easier and faster solution, he first one.
</p>


<p>
  In the constructor of our repository we need to pass an instance of the <strong><em>UnitOfWork</em></strong> that in our case is the Entity Framework, like we do with the session with NHibernate.
</p>


<pre style="border-bottom: #004080 1px solid; border-left: #004080 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #004080 1px solid; border-right: #004080 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  1: <span style="color: #0000ff">namespace</span> PrismTutorial.DataLayer {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  2:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> Repository:BaseRepository,IRepository {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  3: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  4:         #region Base Implementation
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  5: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  6:         <span style="color: #0000ff">private</span> <span style="color: #0000ff">bool</span> _disposed;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  7:         <span style="color: #008000">//Here we pass the connection and we flag the contextReused</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  8:         <span style="color: #008000">//so we can use the repository with the using clause ...</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  9:         <span style="color: #0000ff">public</span> Repository(ADVConnection context) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 10:             <span style="color: #0000ff">this</span>._context = context;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 11:             <span style="color: #0000ff">this</span>._contextReused = <span style="color: #0000ff">true</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 12:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 13: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 14:         #endregion
</pre>


<p>
  Then we have also to implement the IDisposable interface in order to clean-up the connection after each call:
</p>


<pre style="border-bottom: #004080 1px solid; border-left: #004080 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #004080 1px solid; border-right: #004080 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  1:         #region Disposable
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  2:         <span style="color: #008000">//Dispose implementation</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  3:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> Dispose() {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  4:             DisposeObject(<span style="color: #0000ff">true</span>);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  5:             GC.SuppressFinalize(<span style="color: #0000ff">this</span>);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  6:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  7:         <span style="color: #008000">//Distructor</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  8:         ~Repository() {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  9:             DisposeObject(<span style="color: #0000ff">false</span>);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 10:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 11:         <span style="color: #008000">//Concrete private implementation of the dispose method</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 12:         <span style="color: #0000ff">private</span> <span style="color: #0000ff">void</span> DisposeObject(<span style="color: #0000ff">bool</span> disposing) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 13:             <span style="color: #0000ff">if</span> (_disposed) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 14:                 <span style="color: #0000ff">return</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 15:             }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 16:             <span style="color: #0000ff">if</span> (disposing) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 17:                 <span style="color: #0000ff">if</span> (_context != <span style="color: #0000ff">null</span>) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 18:                     _context.Dispose();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 19:                 }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 20:                 _disposed = <span style="color: #0000ff">true</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 21:             }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 22:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 23:         #endregion</pre>


<p>
  Now we can go ahead and implement each C.R.U.D. method and reflects the changes in the entity framework.
</p>


<p>
  The save method is pretty straightforward, we add an object to our context, using itâ€™s <strong><em>FullTypeName and we persist the changes in the database. </em></strong>
</p>


<pre style="border-bottom: #004080 1px solid; border-left: #004080 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #004080 1px solid; border-right: #004080 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  1:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">int</span> Add&lt;T&gt;(T entity) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  2:             ADVConnection context = GetObjectContext();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  3:             context.AddObject(<span style="color: #0000ff">typeof</span>(T).Name, entity);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  4:             <span style="color: #0000ff">int</span> result = context.SaveChanges();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  5:             ReleaseObjectContextIfNotReused();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  6:             <span style="color: #0000ff">return</span> result;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  7:         }</pre>


<p>
  Same for the count methods except that we have to create an <strong><em>ObjectQuery</em></strong> that itâ€™s nothing more than a translated query using the T object passed as a parameter.
</p>


<pre style="border-bottom: #004080 1px solid; border-left: #004080 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #004080 1px solid; border-right: #004080 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  1:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">long</span> Count&lt;T&gt;() {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  2:             ADVConnection context = GetObjectContext();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  3:             var query = <span style="color: #0000ff">new</span> ObjectQuery&lt;T&gt;(
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  4:                 <span style="color: #0000ff">typeof</span>(T).Name, 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  5:                 context, 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  6:                 MergeOption.NoTracking);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  7:             <span style="color: #0000ff">int</span> count = query.Count();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  8:             ReleaseObjectContextIfNotReused();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px">  9:             <span style="color: #0000ff">return</span> count;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 10:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 11: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 12:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">long</span> Count&lt;T&gt;(Expression&lt;Func&lt;T, <span style="color: #0000ff">bool</span>&gt;&gt; expression) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 13:             ADVConnection context = GetObjectContext();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 14:             var query = <span style="color: #0000ff">new</span> ObjectQuery&lt;T&gt;(
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 15:                 <span style="color: #0000ff">typeof</span>(T).Name, 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 16:                 context, 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 17:                 MergeOption.NoTracking)
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 18:                 .Where(expression);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 19:             <span style="color: #0000ff">int</span> count = query.Count();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 20:             ReleaseObjectContextIfNotReused();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 21:             <span style="color: #0000ff">return</span> count;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 22:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 11px"> 23: </pre>


<p>
  The only part that you may not know is the parameter <strong><em>MergeOption </em></strong>used in the objectquery constructor. MergeOption has the following values (from the source of the NET Framework) :
</p>


<pre style="border-bottom: #004080 1px solid; border-left: #004080 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #004080 1px solid; border-right: #004080 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px">  1: <span style="color: #0000ff">namespace</span> System.Data.Objects {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px">  2:     <span style="color: #008000">// Summary:</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px">  3:     <span style="color: #008000">//     Specifies how objects being loaded into the object context are merged with</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px">  4:     <span style="color: #008000">//     objects already in the object context.</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px">  5:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">enum</span> MergeOption {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px">  6:         <span style="color: #008000">// Summary:</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px">  7:         <span style="color: #008000">//     Objects that already exist in the object context are not loaded from the</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px">  8:         <span style="color: #008000">//     persisted store. This is the default behavior for queries or when calling</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px">  9:         <span style="color: #008000">//     the System.Data.Objects.DataClasses.EntityCollection&lt;TEntity&gt;.Load(System.Data.Objects.MergeOption)</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 10:         <span style="color: #008000">//     method on an System.Data.Objects.DataClasses.EntityCollection&lt;TEntity&gt;.</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 11:         AppendOnly = 0,
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 12:         <span style="color: #008000">//</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 13:         <span style="color: #008000">// Summary:</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 14:         <span style="color: #008000">//     Objects are always loaded from the persisted store. Any property changes</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 15:         <span style="color: #008000">//     made to objects in the object context are overwritten by the store values.</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 16:         OverwriteChanges = 1,
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 17:         <span style="color: #008000">//</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 18:         <span style="color: #008000">// Summary:</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 19:         <span style="color: #008000">//     When an object exists in the object context, it is not loaded from the persisted</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 20:         <span style="color: #008000">//     store. Any property changes made to objects in the object context are preserved.</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 21:         <span style="color: #008000">//     This is used to force changes to objects in the object context to save successfully</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 22:         <span style="color: #008000">//     after an System.Data.OptimisticConcurrencyException has occurred. For more</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 23:         <span style="color: #008000">//     information, see Saving Changes and Managing Concurrency (Entity Framework).</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 24:         PreserveChanges = 2,
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 25:         <span style="color: #008000">//</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 26:         <span style="color: #008000">// Summary:</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 27:         <span style="color: #008000">//     Objects are maintained in a System.Data.EntityState.Detached state and are</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 28:         <span style="color: #008000">//     not tracked in the System.Data.Objects.ObjectStateManager.</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 29:         NoTracking = 3,
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 30:     }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 31: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 8px"> 32: </pre>


<p>
  Then we need to implement the <strong>delete</strong> command. Because we are working on a <strong>disconnected</strong> environment, first of all we need to <strong>get </strong>the original object from the repository. Then, we delete it.
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 540px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">int</span> Delete&lt;T&gt;(T entity) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     ADVConnection context = GetObjectContext();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     <span style="color: #0000ff">object</span> originalItem;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:     EntityKey key = context.CreateEntityKey(<span style="color: #0000ff">typeof</span>(T).Name, entity);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:     <span style="color: #0000ff">if</span>(context.TryGetObjectByKey(key, <span style="color: #0000ff">out</span> originalItem)){
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:         context.DeleteObject(originalItem);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:     <span style="color: #0000ff">int</span> result =  context.SaveChanges();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:     ReleaseObjectContextIfNotReused();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:     <span style="color: #0000ff">return</span> result;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12: </pre>


<p>
  Now the easy part, GetAll and GetAll using an expression for the criteria.
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 540px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> IList&lt;T&gt; GetAll&lt;T&gt;() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     ADVConnection context = GetObjectContext();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     IList&lt;T&gt; list = context
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:         .CreateQuery&lt;T&gt;(
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:         "<span style="color: #8b0000">[</span>" + <span style="color: #0000ff">typeof</span>(T).Name + "<span style="color: #8b0000">]</span>")
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:         .ToList();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:     ReleaseObjectContextIfNotReused();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:     <span style="color: #0000ff">return</span> list;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10: </pre>


<p>
  With the criteria:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 540px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> IList&lt;T&gt; GetAll&lt;T&gt;(Expression&lt;Func&lt;T, <span style="color: #0000ff">bool</span>&gt;&gt; expression) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     ADVConnection context = GetObjectContext();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     IList&lt;T&gt; list = context
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:         .CreateQuery&lt;T&gt;(
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:         "<span style="color: #8b0000">[</span>" + <span style="color: #0000ff">typeof</span>(T).Name + "<span style="color: #8b0000">]</span>")
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:         .Where(expression)
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:         .ToList();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:     ReleaseObjectContextIfNotReused();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:     <span style="color: #0000ff">return</span> list;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11: </pre>


<p>
  Just a little note here. If you watch this piece of code: 
</p>


<p>
  <strong><em>.CreateQuery<T>(<br />
        <br />                &#8220;[&#8221; + typeof(T).Name + &#8220;]&#8221;) </em></strong>here I am using the CreateQuery method to get an ObjectQuery object using the generics. Unfortunately this method requires the name of the entity, so we use Reflection and get the name with the method typeof.
</p>


<p>
  The GetSingle method is the same, we just retreive the FirstOrDefault result of our query:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 540px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> T GetSingle&lt;T&gt;(Expression&lt;Func&lt;T, <span style="color: #0000ff">bool</span>&gt;&gt; expression) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     ADVConnection context = GetObjectContext();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     T result = context
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:         .CreateQuery&lt;T&gt;(
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:         "<span style="color: #8b0000">[</span>" + <span style="color: #0000ff">typeof</span>(T).Name + "<span style="color: #8b0000">]</span>")
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:         .Where(expression)
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:         .FirstOrDefault();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:     ReleaseObjectContextIfNotReused();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:     <span style="color: #0000ff">return</span> result;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11: </pre>


<p>
  And finally, the update method that is going to save the changes we did to an entity.
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 540px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">int</span> Update&lt;T&gt;(T entity) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     ADVConnection context = GetObjectContext();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     <span style="color: #0000ff">object</span> originalItem;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:     EntityKey key = context.CreateEntityKey(<span style="color: #0000ff">typeof</span>(T).Name, entity);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:     <span style="color: #0000ff">if</span>(context.TryGetObjectByKey(key, <span style="color: #0000ff">out</span> originalItem)){
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:         context.ApplyPropertyChanges(<span style="color: #0000ff">typeof</span>(T).Name,entity);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:     <span style="color: #0000ff">int</span> result = context.SaveChanges();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:     ReleaseObjectContextIfNotReused();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:     <span style="color: #0000ff">return</span> result;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12: </pre>


<p>
  Like we did with the delete method, first of all we need to retrieve the original instance of the entity, then we simply apply the changes we got from the â€œworkingâ€ entity, using the method <strong><em>ApplyPropertyChanges</em></strong> and finally we save everything.
</p>


<h3>
  Conclusions.
</h3>


<p>
  In this article we saw how to work with the entity framework and build a repository pattern around it. The next step will be to include <strong>business and value validation</strong> to our entities. We will use the enterprise library 4.1.
</p>


<p>
  Stay tuned!
</p>


<p>
  Tags: <a href="http://technorati.com/tag/WPF" rel="tag">WPF</a> <a href="http://technorati.com/tag/Prism" rel="tag">Prism</a> <a href="http://technorati.com/tag/Entity Framework" rel="tag">Entity Framework</a> <a href="http://technorati.com/tag/WCF" rel="tag">WCF</a>
</p>