---
id: 234
title: Build enterprise application with WPF, WCF, Entity Framework and Prism. Tutorial 06.
date: 2009-11-08T17:13:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=234
permalink: /archive/2009/11/08/build-enterprise-application-with-wpf-wcf-entity-framework-and-prism-tutorial-06.aspx
categories:
  - 'C#'
  - Design Pattern
  - WCF
  - WPF
---
### Configure your Customer lookup and run it on IIS 7.0

In the previous article we saw how to use WCF (a basic approach) and what we should keep in consideration if we want to use SOA as our repository.

Now we need to:

  1. Change the WCF service to point to a real database 
  2. Test the service 
  3. Build a web site to host our service 
  4. Host the web site on IIS 7.0 
  5. The the final result 

### Change the Customer service to reflect our database.

First of all letâ€™s open the ServiceLibrary project and change the ICustomerService interface to reflect this:

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: [ServiceContract]
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: <span style="color: #0000ff">interface</span> ICustomerService {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:  [OperationContract]
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:  IList&lt;CustomerDto&gt; GetCustomers();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:  [OperationContract]
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:  IList&lt;CustomerDto&gt; GetFilteredCustomers(<span style="color: #0000ff">string</span> searchCriteria);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10: }</pre>


<p>
  For our menu we need two methods. The first one will retrieve all the available customers, in alphabetical order; the second one will filter this results, in order to show us only the customers that match our search criteria.
</p>


<p>
  The concrete implementation of this contract will consequently change in this way:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: List&lt;CustomerDto&gt; Customers = <span style="color: #0000ff">new</span> List&lt;CustomerDto&gt;();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: IRepository customerRepository = 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:    <span style="color: #0000ff">new</span> Repository(<span style="color: #0000ff">new</span> ADVConnection());</pre>


<p>
  Of course, in order to declare our Repository we need to add a reference to the DataLayer project in our WCF service. We need also to reference the entity framework .dll â€œSystem.data.entityâ€ and we need also to add the <strong>connectionString</strong> section in the app.config of our WCF service, otherwise when we will instantiate a new database session (<strong><em>new ADVConnection())</em></strong> the Visual Studio will throw an error â€œ<em>configuration not found â€¦ </em>â€<em>. This happens because the WCF is the final layers so you can use in .NET just one config file in the final layer (UI)</em>.
</p>


<p>
  Now, what we want to do, is to populate the list of customer, in our service library, with the customers available in the database. Because our service is a singleton, we will do that when the first user will call the service:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> CustomerService() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     var result = from c <span style="color: #0000ff">in</span> customerRepository.GetAll&lt;Customer&gt;() orderby c.FirstName, c.LastName select c;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     <span style="color: #0000ff">foreach</span> (var customer <span style="color: #0000ff">in</span> result) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:         Customers.Add(
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:             <span style="color: #0000ff">new</span> CustomerDto() { 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:                 Id = customer.CustomerID, 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:                 FullName = customer.FirstName + "<span style="color: #8b0000"> </span>" + customer.LastName
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:             }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:         );
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:     }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13: </pre>


<p>
  Now, this piece of code is pretty easy and ugly. We should use something like <a href="http://automapper.codeplex.com/" target="_blank">AutoMapper</a> to populate on fly our DTO but I want to show you  exactly what happens behind the scene. 
</p>


<p>
  We takes all the available customers from the database and one by one, we fill up the Dto with the resultset. 
</p>


<p>
  A niece solution here would be also to use an extension method and do something like â€œ<em>from c in customers select c.ToDto()</em>â€<em> </em>that may returns a IList<CustomerDto>. <img src="http://raffaeu.com/wp-content/uploads/2013/03/29e77e95-99d9-41a0-9d0e-c2a528f11268smiley-wink.gif" border="0" alt="Wink" />
</p>


<p>
  The two methods will change consequently in this way:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> IList&lt;CustomerDto&gt; GetCustomers() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     <span style="color: #0000ff">return</span> Customers;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: }</pre>


<p>
  And the filtered version will change in this way:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> IList&lt;CustomerDto&gt; GetFilteredCustomers(<span style="color: #0000ff">string</span> searchCriteria) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:  <span style="color: #0000ff">return</span> Customers.FindAll(
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:   <span style="color: #0000ff">delegate</span>(CustomerDto c) { 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:    <span style="color: #0000ff">return</span> c.FullName.ToLower()
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:     .Contains(searchCriteria.ToLower()); 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:   }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:  );
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9: </pre>


<p>
  We just said to the List â€œ<em>Hey, looks inside the items and whether the items lower case contains this word, keep it</em>â€<em>. </em>Using this approach will allow us to keep clean the in memory list of customers and retrieve only the customers that match the criteria.
</p>


<h3>
  Test the environment.
</h3>


<p>
  Now if we press F5 our project will compile but when we will try to call the GetCustomers, we will receive this error:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_F24E/image.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_F24E/image_thumb.png" width="260" height="167" /></a>Visual Studio is pretty nasty in this, because whatever you will say in the app.config, it will use a different app.config â€œon flyâ€ when you test your service. So in the service windows, click on the app.config under the customer service and change it in this way:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_F24E/image_3.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_F24E/image_thumb_3.png" width="260" height="193" /></a>By default WCF doesnâ€™t allow to carry out more thanÂ  few bytes on our messages, but because we are retrieving a list of 800 entities â€¦ we should increase this parameter. <font color="#ff0000"><br />
      <br /><strong>Remember that everything has a cost in resources and it is not a good practice to send and receive a lot of megabytes of message content</strong>. </font><font color="#000000">Consider using <em>pagination</em> and other tricks.</font>
</p>


<p>
  Now we need to expose this service on a web site. Why? Because when we will develop the UI we will add a reference to an host web site service, like <em><a href="http://raffaeu.com/service/Customer.svc">http://raffaeu.com/service/Customer.svc</a></em> and not to a dev address like <a href="http://localhost:9080/PrismTutorial"><em>http://localhost:9080/PrismTutorial</em></a><em>â€¦ </em><img src="http://raffaeu.com/wp-content/uploads/2013/03/4196d6fb-3c69-4997-89dc-47aeb8faad90smiley-smile.gif" border="0" alt="Smile" />
</p>


<h3>
  Create a host web site and install it on IIS 7.
</h3>


<p>
  We need to add a new web site application on our solution, but it has to be a WCF Service web site solution, like the picture below:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_F24E/image_4.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_F24E/image_thumb_4.png" width="260" height="175" /></a>Delete all the content except the service.svc file and the web.config.
</p>


<p>
  First add a new reference to our project ServiceLibrary.<br />
    <br />Then add the connection string to the web.config file.
</p>


<p>
  Finally, change the service.svc to customer.svc and change the HTML code in this way:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="background-color: #ffff00; color: black">&lt;%@ 
</span></pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:    ServiceHost Language="C#" 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:    Debug="true" 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:    Service="PrismTutorial.ServiceLibrary.CustomerService" 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: %&gt;</pre>


<p>
  Cool, now letâ€™s modify the web.config of our WCF web site and we are done.
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_F24E/image_5.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_F24E/image_thumb_5.png" width="260" height="178" /></a>I just said to the new endpoint to point to our .dll service library and to expose the contract IServiceContract. The same step we did previously to the service library layer.
</p>


<p>
  If you now press F5 you can see the process running under ASP.NET. Of course we need to install it on IIS in order to have a common address for our future services. Because the web site points to our project, every time we will change the WCF library this will automatically change in the web service. Of course when we will create new contracts we will need to define new <strong>endpoint </strong>in both layers.
</p>


<blockquote style="border-bottom: black 1px dashed; border-left: black 1px dashed; padding-bottom: 5px; background-color: #f4f4f4; margin: 5px; padding-left: 5px; padding-right: 5px; border-top: black 1px dashed; border-right: black 1px dashed; padding-top: 5px">
  <p>
    Note: if you have, like me, Windows 7 and you want to follow this step, you need to install and configure IIS 7 and WCF for IIS. You can do that by following <a href="http://gasparnagy.blogspot.com/2009/01/enable-wcf-hosing-in-iis-7.html" target="_blank">this simple and useful post</a>.
  </p>
  
</blockquote>


<p>
  Open IIS 7 and install the new application by <strong>creating a new application</strong> that will point to our project:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_F24E/image_6.png" rel="lightbox[tutorial]"><img style="border-bottom: 0px; border-left: 0px; display: block; float: none; margin-left: auto; border-top: 0px; margin-right: auto; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_F24E/image_thumb_6.png" width="260" height="186" /></a> If everything is ok, you will be able to browse the folder of this web.app, click on the .svc file, and select <strong>Browse</strong>.
</p>


<p>
  You should get this result:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_F24E/image_7.png" rel="lightbox[tutorial]"><img style="border-bottom: 0px; border-left: 0px; display: block; float: none; margin-left: auto; border-top: 0px; margin-right: auto; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_F24E/image_thumb_7.png" width="260" height="163" /></a>
</p>


<h3>
  Considerations.
</h3>


<p>
  This tutorial shows how to create and host a WCF service using Visual Studio and IIS 7. Starting from the next tutorial I will not show you anymore this part as you can use this article as a reference. 
</p>


<p>
  We will create new services and contracts and we will call them asynchronously inside WPF UI.
</p>


<p>
  I use this approach to get the data on my applications as I can manage better the points of failure of my applications and keep the code separated and clean. If something change on the customer service I can simply add a new method and all the previous versions of my software will continue to work. If my WCF service crashes I can change the web app to point to a different .dll and everything will continue to work. 
</p>


<p>
  Stay tuned as from the next time we will start to build the UI. We will talk about Prism, regions IoC and more.
</p>


<p>
  Tags: <a href="http://technorati.com/tag/WCF" rel="tag">WCF</a> <a href="http://technorati.com/tag/WPF" rel="tag">WPF</a> <a href="http://technorati.com/tag/Prism" rel="tag">Prism</a> <a href="http://technorati.com/tag/Composite application" rel="tag">Composite application</a>
</p>