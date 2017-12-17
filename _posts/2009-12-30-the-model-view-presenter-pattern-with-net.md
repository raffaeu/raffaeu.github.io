---
id: 228
title: The Model View Presenter pattern with .NET.
date: 2009-12-30T13:43:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=228
permalink: /archive/2009/12/30/the-model-view-presenter-pattern-with-net.aspx
categories:
  - 'C#'
  - Design Pattern
  - NET World
---
I had a request from one of my blogâ€™s reader (**Bastien**) that I canâ€™t skip. Last week he asked me to write a short article on how to implement the Model View Presenter with Windows form. So letâ€™s skip for a while <a href="http://blog.raffaeu.com/archive/2009/09/20/build-enterprise-application-with-wpf-wcf-entity-framework-and-prism.aspx" target="_blank">my</a> <a href="http://blog.raffaeu.com/archive/2009/11/08/build-enterprise-application-with-wpf-wcf-entity-framework-and-prism-once-more.aspx" target="_blank">series</a> of <a href="http://blog.raffaeu.com/archive/2009/11/08/build-enterprise-application-with-wpf-wcf-entity-framework-and-prism-to-beat-a-dead-horse.aspx" target="_blank">articles</a> about Prism and letâ€™s see what is the MVP and how it should be implemented.

### The Model View Presenter pattern.

The definition of the model view presenter pattern can be found in many web site, letâ€™s use the classic definition provided by MSDN. (<a href="http://msdn.microsoft.com/en-us/magazine/cc188690.aspx" target="_blank">This is an interesting article that explains the MVP</a>).

<blockquote style="border-bottom: gray 1px solid; border-left: gray 1px solid; padding-bottom: 3px; padding-left: 3px; padding-right: 3px; background: #efefef; border-top: gray 1px solid; border-right: gray 1px solid; padding-top: 3px">
  <p>
    Model-view-presenter (MVP) is a user interface design pattern engineered to facilitate automated unit testing and improve the <a href="http://en.wikipedia.org/wiki/Separation_of_concerns">separation of concerns</a> in presentation logic. <br />1) The model is an interface defining the data to be displayed or otherwise acted upon in the user interface. <br />2) The view is an interface that displays data (the model) and routes user commands (events) to the presenter to act upon that data. <br />3) The presenter acts upon the model and the view. It retrieves data from repositories (the model), persists it, and formats it for display in the view.
  </p>
</blockquote>

There are two different types of MVP implementation: the MVP passing view and the MVP supervising controller. The main difference from these two types of UI patterns is that in the passive view, the view doesnâ€™t know anything about the model and the presenter is in charge of â€œadvise the viewâ€ that something happened. In the second implementation, the view knows the model for the basic binding and the presenter is used only when the view requires complex UI manipulation. 

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TheModelViewPresenterpatternwith.NET_C0E7/image.png" rel="lightbox[MVP]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TheModelViewPresenterpatternwith.NET_C0E7/image_thumb.png" width="370" height="176" /></a> 

Personally I completely disagree with the second approach, because the layers are not really recyclable, and a change in the model constraint the developer to change also the view â€¦ <img src="http://raffaeu.com/wp-content/uploads/2013/03/a42f6d9b-b764-4786-9107-105fa7384fa1smiley-frown.gif" border="0" alt="Frown" />

### The sample model.

As I usually do, you will find a sample application at the end of this article. So letâ€™s start building a very simple fake model that will be the â€œ**famousâ€** Customer-> Orders.   
First of all we need a new Visual Studio solution and we need to structure our application into 3 layers. Of course I am going to create just one project with 3 folders but usually you should have 3 different .dll (UI, Presenter, DAL, Model â€¦) maybe more than 3. <img src="http://raffaeu.com/wp-content/uploads/2013/03/ac9270e5-52c4-406e-9d07-a73092ba0125smiley-tongue-out.gif" border="0" alt="tongue-out" />

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TheModelViewPresenterpatternwith.NET_C0E7/image_3.png" rel="lightbox[mvp]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TheModelViewPresenterpatternwith.NET_C0E7/image_thumb_3.png" width="420" height="193" /></a> 

As you can see from the model we have our classic **DomainObject** class, abstract, that represents a domain object, so in my case I want to know for each object, its unique id and if itâ€™s valid.

Then we have a **Customer**, that can have one or more address, a **List of Order** and for each **Order** a list of **Order Items**.

### The view and the windows forms.

Using the Windows form approach, our view is what we are showing to the user. So in this case our view will be a Windows form. We have 2 views, one is an overview of all the available customers, and the second one is specific to an order.

<p align="center">
  <strong>All available Customers.</strong>
</p>

<p align="center">
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TheModelViewPresenterpatternwith.NET_C0E7/image_4.png" rel="lightbox[mvp]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TheModelViewPresenterpatternwith.NET_C0E7/image_thumb_4.png" width="260" height="205" /></a>
</p>

<p align="center">
  <strong>All available Order for a specific Customer.</strong>
</p>

<p align="center">
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TheModelViewPresenterpatternwith.NET_C0E7/image_5.png" rel="lightbox[MVP]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TheModelViewPresenterpatternwith.NET_C0E7/image_thumb_5.png" width="260" height="195" /></a>
</p>

Now we need to create a generic implementation of the relation View-Model-Presenter and try to recycle some code. This is my idea: we will have a base view that will expose the view name and a couple of methods, then each view will inherits the base one. In this case the view will expose directly the domain entity, but **this should not be done in production, I would suggest to use the DTO**.

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TheModelViewPresenterpatternwith.NET_C0E7/image_6.png" rel="lightbox[MVP]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TheModelViewPresenterpatternwith.NET_C0E7/image_thumb_6.png" width="420" height="245" /></a> The implementation of the Generic View.

Under the folder views, we need to create a new interface, **IBaseView**:

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 400px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">using</span> System;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: <span style="color: #0000ff">using</span> System.Collections.Generic;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: <span style="color: #0000ff">using</span> System.Linq;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: <span style="color: #0000ff">using</span> System.Text;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6: <span style="color: #0000ff">namespace</span> ModelViewPresenter.View {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">interface</span> IBaseView {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:         <span style="color: #0000ff">string</span> Name { <span style="color: #0000ff">get</span>; <span style="color: #0000ff">set</span>; }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10: }</pre>


<p>
  Now we can start to implement the <strong>CustomersView</strong> that will represent the information we have in the Customer window:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 400px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">using</span> System;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: <span style="color: #0000ff">using</span> System.Collections.Generic;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: <span style="color: #0000ff">using</span> System.Linq;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: <span style="color: #0000ff">using</span> System.Text;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: <span style="color: #0000ff">using</span> ModelViewPresenter.Model;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7: <span style="color: #0000ff">namespace</span> ModelViewPresenter.View {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">interface</span> ICustomersView : IBaseView {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:         Customer SelectedCustomer { <span style="color: #0000ff">get</span>; }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:         IList&lt;Customer&gt; Customers { <span style="color: #0000ff">get</span>; <span style="color: #0000ff">set</span>; }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12:         <span style="color: #0000ff">string</span> FirstName { <span style="color: #0000ff">set</span>; }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13:         <span style="color: #0000ff">string</span> LastName { <span style="color: #0000ff">set</span>; }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 14:         <span style="color: #0000ff">string</span> BirthDate { <span style="color: #0000ff">set</span>; }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 15:         <span style="color: #0000ff">string</span> Address { <span style="color: #0000ff">set</span>; }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 16:     }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 17: }</pre>


<p>
  In this view we have two different types of information. First of all we have an IList of Customer that will represent our data source for the main grid, and a single instance of type Customer that will represent the selected Customer in the grid.
</p>


<p>
  Then we have some basic data type information that we will use to render the current selected Customer in the grid. These information will be filled by the presenter. Unfortunately in my case the view knows the model but you can avoid that using a Dto.
</p>


<h3>
  A fake repository.
</h3>


<p>
  Of course we usually have a DAL that read the data from the Db and translate these information into a collection of entities, or better, inside our Domain Model. For this application I have created a simple repository structure just to show you how I implement the MVP pattern using generics.
</p>


<p>
  The base repository:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 400px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">using</span> System;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: <span style="color: #0000ff">using</span> System.Collections.Generic;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: <span style="color: #0000ff">using</span> System.Linq;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: <span style="color: #0000ff">using</span> System.Text;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6: <span style="color: #0000ff">namespace</span> ModelViewPresenter.Model {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">interface</span> IBaseRepository {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:         <span style="color: #0000ff">void</span> Initialize();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10: }</pre>


<p>
  And then the concrete implementation for our views:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 400px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">using</span> System;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: <span style="color: #0000ff">using</span> System.Collections.Generic;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: <span style="color: #0000ff">using</span> System.Linq;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: <span style="color: #0000ff">using</span> System.Text;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6: <span style="color: #0000ff">namespace</span> ModelViewPresenter.Model {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">sealed</span> <span style="color: #0000ff">class</span> CustomersRepository : IBaseRepository {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:         <span style="color: #0000ff">private</span> IList&lt;Customer&gt; customers;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:         
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11:         #region IBaseRepository Members
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> Initialize() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 14:             customers = <span style="color: #0000ff">new</span> List&lt;Customer&gt;();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 15:         }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 16: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 17:         #endregion
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 18: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 19:         <span style="color: #0000ff">public</span> IList&lt;Customer&gt; GetCustomers() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 20:             customers.Add(
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 21:             ... ...
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 22: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 23:             <span style="color: #0000ff">return</span> customers;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 24:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 25: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 26:         <span style="color: #0000ff">private</span> <span style="color: #0000ff">void</span> AddAddressToCustomer(Customer customer) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 27:             ... ...
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 28:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 29: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 30:         <span style="color: #0000ff">private</span> <span style="color: #0000ff">void</span> AddOrdersToTheCustomer(Customer customer) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 31:             ... ...
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 32:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 33:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 34: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 35: </pre>


<p>
  My code doesnâ€™t do anything special. It just creates a fake collection of customers and will add 2 orders with some order items for each one.
</p>


<h3>
  Use some glue and put all together, the presenter.
</h3>


<p>
  Now itâ€™s time to play with a generic presenter that wonâ€™t know the view and or the repository. My idea is to do something like this:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 400px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">using</span> System;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: <span style="color: #0000ff">using</span> System.Collections.Generic;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: <span style="color: #0000ff">using</span> System.Linq;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: <span style="color: #0000ff">using</span> System.Text;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: <span style="color: #0000ff">using</span> ModelViewPresenter.View;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6: <span style="color: #0000ff">using</span> ModelViewPresenter.Model;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8: <span style="color: #0000ff">namespace</span> ModelViewPresenter.Presenter {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">abstract</span> <span style="color: #0000ff">class</span> BasePresenter&lt;TView, TRepository&gt; 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:            where TView : IBaseView 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11:            where TRepository : IBaseRepository {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12:         
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13:         <span style="color: #0000ff">protected</span> TView view;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 14:         <span style="color: #0000ff">protected</span> TRepository repository;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 15: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 16:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">virtual</span> <span style="color: #0000ff">void</span> InitializeView() { }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 17: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 18:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">virtual</span> <span style="color: #0000ff">void</span> UpdateView(<span style="color: #0000ff">object</span> model) { }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 19:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 20: }</pre>


<p>
  First of all we have to declare a generic view and a generic repository. In this way we will pass our concrete view and repository when we will create a new instance of the presenter. Then I have added to simple virtual methods. The first one will â€œprepareâ€ the view, so we can call it in the load method of the form, or better, in his constructor. UpdateView will receive the selected â€œinstanceâ€Â  and will update the view.
</p>


<p>
  Letâ€™s try to imagine how the CustomersPresenter should be implemented following this approach:
</p>


<p>
  We declare the class
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 400px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">sealed</span> <span style="color: #0000ff">class</span> CustomersPresenter : 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:      BasePresenter&lt;ICustomersView, CustomersRepository&gt; {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: </pre>


<p>
  The we implement the 2 constructors:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 400px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> CustomersPresenter(ICustomersView view) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:  repository = <span style="color: #0000ff">new</span> CustomersRepository();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:  repository.Initialize();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:  <span style="color: #0000ff">this</span>.view = view;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7: <span style="color: #0000ff">public</span> CustomersPresenter(ICustomersView view, CustomersRepository repository) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:  <span style="color: #0000ff">this</span>.view = view;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:  <span style="color: #0000ff">this</span>.repository = repository;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:  repository.Initialize();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12: </pre>


<p>
  And then we start to write the code that will override the default methods of our presenter:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 400px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">override</span> <span style="color: #0000ff">void</span> InitializeView() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     <span style="color: #0000ff">base</span>.InitializeView();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     <span style="color: #0000ff">this</span>.view.Name = "<span style="color: #8b0000">All available Customers View.</span>";
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:     <span style="color: #0000ff">this</span>.view.Customers = repository.GetCustomers();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6: </pre>


<p>
  and
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 400px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">override</span> <span style="color: #0000ff">void</span> UpdateView(<span style="color: #0000ff">object</span> model) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     <span style="color: #0000ff">base</span>.UpdateView(model);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     var selected = model <span style="color: #0000ff">as</span> Customer;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:     <span style="color: #0000ff">if</span> (selected != <span style="color: #0000ff">null</span>) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:         <span style="color: #0000ff">this</span>.view.FirstName = selected.FirstName;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:         <span style="color: #0000ff">this</span>.view.LastName = selected.LastName;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:         <span style="color: #0000ff">this</span>.view.BirthDate = selected.BirthDate.ToShortDateString();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:         <span style="color: #0000ff">this</span>.view.Address = selected.FirstAddress;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11: </pre>


<p>
  When a customer is selected:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 400px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> SelectionChanged() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     UpdateView(<span style="color: #0000ff">this</span>.view.SelectedCustomer);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: </pre>


<p>
  Finally we need to bind all this code in our form, that will â€œimplementâ€ the view and will â€œknowâ€ the presenter. Letâ€™s see.
</p>


<h3>
  Bind the view and the presenter to the corresponding windows form.
</h3>


<p>
  We have previously created a windows form with some controls and a gridview that will show the available customers. Now, first of all, we implement the view in the form:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 400px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1:     <span style="color: #0000ff">public</span> partial <span style="color: #0000ff">class</span> CustomersView : Form, ICustomersView {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:         #region ICustomersView Members
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> FirstName 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:         {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:             <span style="color: #0000ff">set</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:                 txtFisrtName.Text = <span style="color: #0000ff">value</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:             }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> LastName {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13:             <span style="color: #0000ff">set</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 14:                 txtLastName.Text = <span style="color: #0000ff">value</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 15:             }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 16:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 17: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 18:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> BirthDate {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 19:             <span style="color: #0000ff">set</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 20:                 txtDateBirth.Text = <span style="color: #0000ff">value</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 21:             }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 22:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 23: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 24:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> Address {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 25:             <span style="color: #0000ff">set</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 26:                 txtAddress.Text = <span style="color: #0000ff">value</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 27:             }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 28:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 29: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 30:         <span style="color: #0000ff">public</span> Customer SelectedCustomer {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 31:             <span style="color: #0000ff">get</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 32:                 <span style="color: #0000ff">if</span> (grdCustomers.SelectedRows.Count &gt; 0) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 33:                     <span style="color: #0000ff">return</span> (Customer)grdCustomers.SelectedRows[0].DataBoundItem;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 34:                 }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 35:                 <span style="color: #0000ff">return</span> <span style="color: #0000ff">null</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 36:             }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 37:         }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 38: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 39:         <span style="color: #0000ff">public</span> IList&lt;Customer&gt; Customers {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 40:             <span style="color: #0000ff">get</span> {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 41:                 <span style="color: #0000ff">return</span> (IList&lt;Customer&gt;)grdCustomers.DataSource;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 42:             }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 43:             <span style="color: #0000ff">set</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 44:                 grdCustomers.AutoGenerateColumns = <span style="color: #0000ff">false</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 45:                 grdCustomers.DataSource = <span style="color: #0000ff">value</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 46:             }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 47:         }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 48: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 49:         #endregion</pre>


<p>
  And now we can assign the presenter to this view in the following way:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 400px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">protected</span> CustomersPresenter presenter;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: <span style="color: #0000ff">public</span> CustomersView() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:    InitializeComponent();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:    presenter = <span style="color: #0000ff">new</span> CustomersPresenter(<span style="color: #0000ff">this</span>);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:    presenter.InitializeView();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8: </pre>


<p>
  Now, whatever will happen in the form, the corresponding event will call a method in the presenter. In this way we are sure that the form (view) will know only the presenter. Remember that in order to satisfy this prerequisite, you should have a Dto that represents the information you want to show in the view, and not a reference to domain model.
</p>


<p>
  Select a customer:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 400px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">private</span> <span style="color: #0000ff">void</span> grdCustomers_SelectionChanged(<span style="color: #0000ff">object</span> sender, EventArgs e) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:    presenter.SelectionChanged();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: </pre>


<p>
  And show the corresponding orders, a method handled by the presenter that will open a new orders view and show the corresponding orders:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 400px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">private</span> <span style="color: #0000ff">void</span> grdCustomers_DoubleClick(<span style="color: #0000ff">object</span> sender, EventArgs e) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     presenter.LoadOrders();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: </pre>


<h3>
  Conclusions and source code.
</h3>


<p>
  The <strong><u>source code</u></strong> of this article is available on my sky drive at this address. The solution is <strong><u>complete</u></strong> so you will find more code, the implementation of the navigator pattern, useful to navigate through the views, and the complete design of the windows form.
</p>


<p>
  This is the address: <a href="http://cid-ada7c80f0c2f057b.skydrive.live.com/browse.aspx/Visual%20Studio%20Projects?uc=1&isFromRichUpload=1" target="_blank">Raffaeu Visual Studio Sky drive</a>
</p>


<p>
  Remember that you should <strong>never</strong> share your domain entities inside the view, leave them in the presenter and try to use the Dto and a framework like AutoMapper to do the rest.
</p>


<p>
  This code is trivial just to show you how to implement the MVP using generics, from my point of view. It can be implemented in a better way and it can also be refactored.<br />
    <br />I will appreciate if you have suggestions and feedbacks.
</p>


<p>
  If you need another example, like how to use the MVC, or how to implement validation in MVVM, please write me an e-mail, and I will try to satisfy your request.
</p>


<p>
  For now I will continue my series using WPF, Prism and WCF. So stay tuned and Happy new year!
</p>


<p>
  Tags: <a href="http://technorati.com/tag/Design Patterns" rel="tag">Design Patterns</a> <a href="http://technorati.com/tag/Model View Presenter" rel="tag">Model View Presenter</a> <a href="http://technorati.com/tag/C#" rel="tag">C#</a>
</p>