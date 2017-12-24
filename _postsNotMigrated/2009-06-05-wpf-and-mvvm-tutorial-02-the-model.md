---
id: 263
title: WPF and MVVM tutorial 02, The model.
date: 2009-06-05T10:02:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=263
permalink: /archive/2009/06/05/wpf-and-mvvm-tutorial-02-the-model.aspx
categories:
  - 'C#'
  - Design Pattern
  - WPF
---
In the <a href="http://blog.raffaeu.com/archive/2009/06/03/wpf-and-vmmv-tutorial-01-introduction.aspx" target="_blank">first part</a> of this tutorial we saw the MVVM model and how it works.

In this part of our tutorial we will work directly with the Entity Model and LinqToSQL. I am using a database-first approach so in my opinion using LinqToSQL will be better then Entitiy Framework. I am going also to show you an easy way to build a custom Unit Of Work to manage the context status with Linq 2 SQL.

### The Visual Studio Project.

First of all open a blank Visual Studio solution, I called it **WPF.Tutorial.VMMV**. Add two projects on it: 1) WPF Application â€¦UI and 2) Class Library (â€¦DAL).

The final layout should be this one (in my picture the model is already inside).

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandVMMVtutorial02Themodel_A958/NewPicture8.png" rel="lightbox[Tutorial2]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="New Picture (8)" border="0" alt="New Picture (8)" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandVMMVtutorial02Themodel_A958/NewPicture8_thumb.png" width="260" height="250" /></a> 

### The Data model.

If you have installed the Adventure Works database in the past you already know what I am talking about. The model I will use in the tutorial will come up with this layout:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandVMMVtutorial02Themodel_A958/NewPicture7.png" rel="lightbox[Tutorial2]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="New Picture (7)" border="0" alt="New Picture (7)" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandVMMVtutorial02Themodel_A958/NewPicture7_thumb.png" width="260" height="182" /></a>We will have a customer with the personal information and the related orders with the order information. 

Go into the DAL project and from the menu choose **add new file â€“> LinqToSQL** and call it DataModel. Now you need to connect you data model into your AW database and drag in the design pane the tables you need. At the end you should come up with the same layout I have in the previous picture.

### The Unit of Work.

If you do not what I am talking about, this is the <a href="http://www.martinfowler.com/eaaCatalog/unitOfWork.html" target="_blank">explanation by Martin Fowler</a>. 

> _A Unit of Work keeps track of everything you do during a business transaction that can affect the database. When you&#8217;re done, it figures out everything that needs to be done to alter the database as a result of your work._

Of course with LinqToSQL we do not need a Unit of Work because L2S by itself is the data context, but because it is not able to manage the disposing in a good way, here are my 2 cents about.

We need a contract, in this way an interface in the DAL that will implement the Unit of Work:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandVMMVtutorial02Themodel_A958/image.png" rel="lightbox[Tutorial2]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandVMMVtutorial02Themodel_A958/image_thumb.png" width="260" height="249" /></a> 

First of all letâ€™s build the C.R.U.D. implementation into a simple interface. In this case I am going to use the generics and the Linq expression because my project will work only with Linq so I do not need an high level of abstraction:

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">using</span> System;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: <span style="color: #0000ff">using</span> System.Collections.Generic;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: <span style="color: #0000ff">namespace</span> WPF.Tutorials.VMMV.DAL {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">interface</span> IUnitOfWork {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:         <span style="color: #008000">//Basic C.R.U.D. operations</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:         <span style="color: #0000ff">void</span> Add&lt;T&gt;(T _entity) where T : <span style="color: #0000ff">class</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:         <span style="color: #0000ff">void</span> Delete&lt;T&gt;(T _entity) where T : <span style="color: #0000ff">class</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:         <span style="color: #0000ff">void</span> Update&lt;T&gt;(T _entity) where T : <span style="color: #0000ff">class</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:         <span style="color: #0000ff">void</span> Commit();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11:         <span style="color: #008000">//Basic Select operations</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12:         IList&lt;T&gt; GetAll&lt;T&gt;() where T : <span style="color: #0000ff">class</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13:         T GetById&lt;T&gt;(Func&lt;T, <span style="color: #0000ff">bool</span>&gt; _condition) where T : <span style="color: #0000ff">class</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 14:     }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 15: }</pre>


<p>
  Now we need the real Unit of Work that will inherit from our contract. We want also to inherit from IDisposable because otherwise we will not be able to keep alive our datacontext during the crud operations.
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">using</span> System;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: <span style="color: #0000ff">using</span> System.Collections.Generic;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: <span style="color: #0000ff">using</span> System.Linq;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: <span style="color: #0000ff">using</span> System.Text;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6: <span style="color: #0000ff">namespace</span> WPF.Tutorials.VMMV.DAL {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:     <span style="color: #008000">//Inherits IUnitOfWork and IDisposable</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> UnitOfWork:IUnitOfWork,IDisposable {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:         <span style="color: #008000">//A Static instance of the Linq Data Context</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:         <span style="color: #0000ff">private</span> <span style="color: #0000ff">static</span> DataModelDataContext _service;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11:         <span style="color: #008000">//The default constructor</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12:         <span style="color: #0000ff">public</span> UnitOfWork() {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13:             _service = <span style="color: #0000ff">new</span> DataModelDataContext();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 14:         }</pre>


<p>
  Now, the second step is to <strong><em>translate</em></strong> the UoW in something readable for Linq:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #008000">//Add a new entity to the model</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> Add&lt;T&gt;(T _entity) where T: <span style="color: #0000ff">class</span> {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     var table = _service.GetTable&lt;T&gt;();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:     table.InsertOnSubmit(_entity);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6: <span style="color: #008000">//Delete an existing entity from the model</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7: <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> Delete&lt;T&gt;(T _entity) where T: <span style="color: #0000ff">class</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:     var table = _service.GetTable&lt;T&gt;();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:     table.DeleteOnSubmit(_entity);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11: <span style="color: #008000">//Update an existing entity</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12: <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> Update&lt;T&gt;(T _entity) where T : <span style="color: #0000ff">class</span> {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13:     var table = _service.GetTable&lt;T&gt;();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 14:     table.Attach(_entity,<span style="color: #0000ff">true</span>);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 15: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 16: <span style="color: #008000">//Commit all the pending changes in the data context</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 17: <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> Commit() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 18:     _service.SubmitChanges();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 19: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 20: <span style="color: #008000">//Get the entire Entity table</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 21: <span style="color: #0000ff">public</span> IList&lt;T&gt; GetAll&lt;T&gt;()where T : <span style="color: #0000ff">class</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 22:     <span style="color: #0000ff">return</span> _service.GetTable&lt;T&gt;().ToList();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 23: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 24: <span style="color: #008000">//Get the first occurence that reflect the Linq Query</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 25: <span style="color: #0000ff">public</span> T GetById&lt;T&gt;(Func&lt;T, <span style="color: #0000ff">bool</span>&gt; _condition) where T : <span style="color: #0000ff">class</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 26:     <span style="color: #0000ff">return</span> _service.GetTable&lt;T&gt;().Where(_condition).FirstOrDefault();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 27: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 28: </pre>


<p>
  Now we have a complete Unit of Work that we will use in each repository. Ops I forgot to mention that we will not have a real repository in our solution but a view model â€¦
</p>


<p>
  Tags: <a href="http://technorati.com/tag/MVVM" rel="tag">MVVM</a> <a href="http://technorati.com/tag/Model View ViewModel" rel="tag">Model View ViewModel</a> <a href="http://technorati.com/tag/WPF" rel="tag">WPF</a>
</p>