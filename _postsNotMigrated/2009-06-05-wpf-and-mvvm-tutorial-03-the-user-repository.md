---
id: 262
title: WPF and MVVM tutorial 03, The user repository.
date: 2009-06-05T16:18:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=262
permalink: /archive/2009/06/05/wpf-and-mvvm-tutorial-03-the-user-repository.aspx
categories:
  - 'C#'
  - Design Pattern
  - WPF
---
Before starting to view in depth our model, or to design the views, I want to complete the DAL layer. Now that we have our unit of work implementation and our data context we need to implement a couple of repositories.

If you want to view how it should work a repository, I suggest <a href="http://martinfowler.com/eaaCatalog/repository.html" target="_blank">this interesting article from Martin Fowler</a>. The **presenter** **should be:**

> _A Repository mediates between the domain and data mapping layers, acting like an in-memory domain object collection._

### The Flow of our application.

During the time I saw different repositories. One for each entity, one for each View and so on. My approach, usually, is to use a relationship of 1-to-1 from the view and the repository. So in our case we will have:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandVMMVtutorial03Therepository_8F3C/image.png" rel="lightbox[Tutorial3]"><img style="border-right-width: 0px; margin: 2px 10px 5px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" align="left" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandVMMVtutorial03Therepository_8F3C/image_thumb.png" width="176" height="260" /></a> 1 &#8211; The user opens the program and has to decide (search a customer, view all customers)

2 â€“ A list will be presented to the user (filtered or not) and he will have to select **one customer**.

3 â€“ A third view will show the details of the user, with two sub-view (address and order details).

So this is the flow that we will follow. For these reasons, you will easily understand that we need a couple of repositories: A) a user repository, B) an order repository.

Â 

### The User repository.

The user repository has to give us a way to execute any kind of CRUD operation against our database. So we should have something like:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandVMMVtutorial03Therepository_8F3C/image_3.png" rel="lightbox[Tutorial3]"><img style="border-right-width: 0px; margin: 2px 10px 5px 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" align="left" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandVMMVtutorial03Therepository_8F3C/image_thumb_3.png" width="257" height="260" /></a> 

Here there are some basic operations we must be able to accomplish with our repository.

1) Get a specific Customer or Get all of them.

2) Add, Update or Drop a Customer.

3)Commit the changes to the data-model.

Â 

Â 

The code is very simple. First of all we need to create a **sealed repository**, so that nobody will _**play** with it**,**_ for this purpose there is the IUnitOfWOrk interface â€¦

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">sealed</span> <span style="color: #0000ff">class</span> CustomerRepository {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     <span style="color: #008000">//Create an istance (it's static) of our Unit of Work</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     <span style="color: #0000ff">private</span> IUnitOfWork _service;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:     <span style="color: #008000">//but let's give it a read-only access</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:     <span style="color: #0000ff">public</span> IUnitOfWork Service {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:         <span style="color: #0000ff">get</span> { <span style="color: #0000ff">return</span> _service; }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:     <span style="color: #008000">//create a new istance of the Unit of work</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:     <span style="color: #0000ff">public</span> CustomerRepository() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:         _service = <span style="color: #0000ff">new</span> UnitOfWork();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12: </pre>


<p>
  Then we need to implement the custom methods to retrieve the information we need from our model. Fortunately we have a <strong><em>generic unit of work</em></strong> so the code behind itâ€™s very very simple!
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #008000">//Add a new customer</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> AddCustomer(Customer _customer) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     Service.Add&lt;Customer&gt;(_customer);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: <span style="color: #008000">//Drop an existing one</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6: <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> DeleteCustomer(Customer _customer) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:     Service.Delete&lt;Customer&gt;(_customer);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9: <span style="color: #008000">//Update an existing dirty customer</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10: <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> UpdateCustomer(Customer _customer) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11:     Service.Update&lt;Customer&gt;(_customer);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13: Get a specific customer
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 14: <span style="color: #0000ff">public</span> Customer GetCustomer(<span style="color: #0000ff">int</span> id) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 15:     <span style="color: #0000ff">return</span> Service.GetById&lt;Customer&gt;(p =&gt; p.CustomerID == id);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 16: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 17: <span style="color: #008000">//Get all of them</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 18: <span style="color: #0000ff">public</span> IList&lt;Customer&gt; GetAllCustomers() {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 19:     <span style="color: #0000ff">return</span> Service.GetAll&lt;Customer&gt;();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 20: }</pre>


<p>
  We should also implement a <strong><em>filtered GetAllCustomers</em></strong> but we will use the View Model to do that in our example.
</p>


<h3>
  Customer class diagram.
</h3>


<p>
  Now we are pretty fine with our Customer model. What we have know is a model, a repository and a CRUD implementation for the customer and the related entities (Address and so on â€¦). The final result in our DAL layer is this one:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandVMMVtutorial03Therepository_8F3C/image_4.png" rel="lightbox[Tutorial3]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandVMMVtutorial03Therepository_8F3C/image_thumb_4.png" width="260" height="175" /></a>
</p>


<p>
  Great we can already build now 3 views (CustomersView, CustomerView, FilteredCustomersView) just by using one repository. TO be honest we should be able to do everything but I want to keep separate the Order section.
</p>


<p>
  In the next step I will give an overview of the ViewModel, and the View interaction, then we will start to build the views and then the relative ViewModels.
</p>


<p>
  <strong><em><font color="#808000">Ndr. I saw that this series of articles is being read by a nice number of people. Please post any ideas, suggestion or whatever you think about â€¦ I will appreciate.</font></em></strong>
</p>


<p>
  Tags: <a href="http://technorati.com/tag/MVVM" rel="tag">MVVM</a> <a href="http://technorati.com/tag/Model View ViewModel" rel="tag">Model View ViewModel</a> <a href="http://technorati.com/tag/WPF" rel="tag">WPF</a>
</p>