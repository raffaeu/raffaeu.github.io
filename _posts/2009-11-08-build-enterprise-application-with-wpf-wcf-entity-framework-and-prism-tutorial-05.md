---
id: 235
title: Build enterprise application with WPF, WCF, Entity Framework and Prism. Tutorial 05.
date: 2009-11-08T10:24:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=235
permalink: /archive/2009/11/08/build-enterprise-application-with-wpf-wcf-entity-framework-and-prism-tutorial-05.aspx
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

### Starting with WCF.

Today we will see how WCF works and what are the best practices we should use in order to keep our application safe and maintainable.

### WCF startup.

I assume that you know already what is WCF and whatâ€™s the difference between WCF and the old Web Service technology. If you donâ€™t, <a href="http://msdn.microsoft.com/en-us/netframework/dd939784.aspx" target="_blank">this is a nice overview of WCF</a>, and <a href="http://www.techbubbles.com/wcf/wcf-vs-aspnet-web-services/" target="_blank">this is a nice overview</a> of the differences from these 2 technologies.

The big difference from using a normal data access layer and a SOA service is in the architecture. We must keep in consideration, **always**, that we are working with a _message service ****_and that all the information are serialized and passed through the network. This point itâ€™s really important because the most common error I saw using WCF is to **serialize** directly the entities in the domain model â€¦ 

Letâ€™s keep as an example our Customer entity.

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_A9BF/image.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_A9BF/image_thumb.png" width="260" height="234" /></a> 

We have a lot of information in this graph and I am pretty sure that we will use those information only when we will look at the details of each customer, so it completely doesnâ€™t make any sense to carry all these information with us for all the time.

Letâ€™s have a break and letâ€™s see what will be the final result of our application using a sketch. (_I use Microsoft <a href="http://www.microsoft.com/expression/products/Blend_Overview.aspx" target="_blank">Expression Blend sketch available for trial here</a>_). The style is modified by me to reflect Balsamiq, another Sketch flow design software.

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_A9BF/myImage.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="myImage" border="0" alt="myImage" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_A9BF/myImage_thumb.png" width="260" height="197" /></a>

What we will do, when the navigation bar will be open to the **Customer** section, we will load a list of Customers, but we need only the **Id** and a **FullName** that will represent the customer. Then if the user will click on a customer, we will load an additional view with all the details. To accomplish this **data transformation** we will use a Dto (Data transfer object) 

<blockquote style="border-bottom: gray 1px dashed; border-left: gray 1px solid; padding-bottom: 5px; background-color: #f4f4f4; margin: 5px; padding-left: 5px; padding-right: 5px; border-top: gray 1px solid; border-right: gray 1px solid; padding-top: 5px">
  <p>
    <i>â€œThe Data Transfer Object &#8220;DTO&#8221;, is a simple serializable object used to transfer data across multiple layers of an application. The fields contained in the DTO are usually primitive types such as strings, boolean, etc. Other DTOs may be contained or aggregated in the DTO.â€ </i>
  </p>
</blockquote>

directly inside WCF. In this way we will have a light version of our customer entity that will be carried over the network.

#### Customer Dto.

Letâ€™s create a new WCF project in our solution and add a new class. The class will be a **serialized light version** of our Customer entity.

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">using</span> System.Runtime.Serialization;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: <span style="color: #0000ff">namespace</span> PrismTutorial.WCF {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:     [DataContract]
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> CustomerDto {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:         [DataMember]
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">int</span> Id { <span style="color: #0000ff">get</span>; <span style="color: #0000ff">set</span>; }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:         [DataMember]
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> FullName { <span style="color: #0000ff">get</span>; <span style="color: #0000ff">set</span>; }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13: </pre>


<p>
  First of all, the <strong>DataContract attribute</strong>. This attribute identifies the entity CustomerDto as <strong>serializable</strong>. Through the attribute <strong>DataMember</strong> we are saying that both the Id and the FullName properties will be serialized.
</p>


<p>
  In order to use this Dto, we need a service <strong>contract</strong> that will allows us to do some simple operations with the Dto. The service contract will expose the operations that we will allow to the end user. 
</p>


<p>
  Letâ€™s add a new interface on our WCF project that will look in this way:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">using</span> System.Collections.Generic;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: <span style="color: #0000ff">using</span> System.ServiceModel;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: <span style="color: #0000ff">namespace</span> PrismTutorial.WCF {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:     [ServiceContract]
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:     <span style="color: #0000ff">interface</span> ICustomerService {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:     
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:         [OperationContract]
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:         <span style="color: #0000ff">void</span> AddCustomer(CustomerDto customer);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11:         
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12:         [OperationContract]
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13:         <span style="color: #0000ff">void</span> DeleteCustomer(CustomerDto customer);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 14:         
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 15:         [OperationContract]
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 16:         IList&lt;CustomerDto&gt; GetCustomers();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 17:     
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 18:     }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 19: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 20: </pre>


<p>
  The two attributes that we use now are <strong>ServiceContract</strong> and <strong>OperationContract</strong>. The first one will identify the Interface as a WCF contract. Using this attribute we are going to say to WCF: â€œHey this is the contract that I want to expose, so letâ€™s look inside and see if there is anything useful for youâ€.
</p>


<p>
  The second attribute is identifying our method as visible to the contract. Of course we can have also some methods that we want to include in the service but that we donâ€™t want to expose to the public.
</p>


<p>
  After that we need to implement in a concrete class our contract and implement the operations. This is just an example so we will have an internal IList of Dto and we will use the operations just to interact with the list exposed by the service.
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">using</span> System;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: <span style="color: #0000ff">using</span> System.Collections.Generic;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: <span style="color: #0000ff">using</span> System.Linq;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: <span style="color: #0000ff">using</span> System.Web;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: <span style="color: #0000ff">using</span> System.ServiceModel;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7: <span style="color: #0000ff">namespace</span> PrismTutorial.WCF {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:     [ServiceBehavior(InstanceContextMode = InstanceContextMode.Single)]
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> CustomerService : ICustomerService {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11:         IList&lt;CustomerDto&gt; Customers = <span style="color: #0000ff">new</span> List&lt;CustomerDto&gt;();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13:         #region ICustomerService Members
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 14: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 15:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> AddCustomer(CustomerDto customer) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 16:             Customers.Add(customer);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 17:         }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 18: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 19:         <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> DeleteCustomer(CustomerDto customer) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 20:             Customers.Remove(customer);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 21:         }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 22: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 23:         <span style="color: #0000ff">public</span> IList&lt;CustomerDto&gt; GetCustomers() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 24:             <span style="color: #0000ff">return</span> Customers;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 25:         }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 26: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 27:         #endregion
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 28:     }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 29: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 30: </pre>


<p>
  The only notable thing here is the attribute <strong>ServiceBehavior </strong>that explains how our service will be created.Â  In this case we said that the first call will activate the service, so itâ€™s like using a <strong><a href="http://www.google.com/url?sa=t&source=web&ct=res&cd=2&ved=0CAsQFjAB&url=http%3A%2F%2Fmsdn.microsoft.com%2Fen-us%2Flibrary%2Fms998558.aspx&ei=hs32Sp-qIcqe8Aa7ysHzCQ&usg=AFQjCNH5TTb6DmFWBzA_KX7dGpX7Oaj_YA&sig2=JBZOzgIqf6PMK_vV2Enb8w" target="_blank">SingleTon</a> pattern</strong>. Of course the service behavior attribute has a lot of options.
</p>


<p>
  We can also use this code in order to be sure that our entity will be correctly removed. Change the intenal IList to a List<T> and change the DeleteCustomer method in this way:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 500px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> DeleteCustomer(CustomerDto customer) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:    Customers.Remove(Customers.Find(
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:       c =&gt; c.Id.Equals(customer.Id))
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:    );
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6: </pre>


<h3>
  Configure the service.
</h3>


<p>
  Now that we have our basic service we have to build the solution CTRL+SHIFT+B and then right click on the app.config and select <strong>Configure.</strong> We will see a window like this one:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_A9BF/image_3.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_A9BF/image_thumb_3.png" width="260" height="193" /></a>First of all click on service one and point to the compile .dll in order to have the service to Customer Service and not Service1.
</p>


<p>
  Now select the endpoint in the service node and again, change it to point to our .dll and select the ICustomerService.
</p>


<p>
  Now if you press F5 and set the WCF as the startup project, you will be prompt by this windows that is the default test window of Visual Studio 2008 for WCF.
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_A9BF/image_4.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: block; float: none; border-top-width: 0px; border-bottom-width: 0px; margin-left: auto; border-left-width: 0px; margin-right: auto" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/BuildenterpriseapplicationwithWPFWCFEnt_A9BF/image_thumb_4.png" width="260" height="172" /></a>By clicking on a method you will be prompted by a default view that allows us to interact with the service in testing mode.
</p>


<p>
  You can test it by using the addCustomer a couple of times and then the GetCustomers. You will find in the message the customers you previously added.
</p>


<h3>
  Considerations.
</h3>


<p>
  In this post we saw how WCF works so now we can do our considerations and create the service for each entity or view, it depends on how you want to structure your software. Of course we should do something better like sending a response message each time we do an operation, or get back the changed list of customers each time we do a CRUD operation. 
</p>


<p>
  The are also other considerations using WCF and attributes that we didnâ€™t see this time. This will be part of the next tutorial.
</p>


<p>
  Finally, I want to give you some tips using WCF that I have found in the pattern and practices on MSDN web site.
</p>


<ul>
  <li>
    Always consider to use versioning with your data contracts<br />
        <br /><a title="http://msdn.microsoft.com/en-us/library/ms733832.aspx" href="http://msdn.microsoft.com/en-us/library/ms733832.aspx">http://msdn.microsoft.com/en-us/library/ms733832.aspx</a> 
  </li>
  
  
  <li>
    Version you contract to avoid â€œbreaking changesâ€<br />
        <br /><a title="http://msdn.microsoft.com/en-us/library/ms731060.aspx" href="http://msdn.microsoft.com/en-us/library/ms731060.aspx">http://msdn.microsoft.com/en-us/library/ms731060.aspx</a> 
  </li>
  
  
  <li>
    Load balancing with WCF<br />
        <br /><a title="http://msdn.microsoft.com/en-us/library/ms730128.aspx" href="http://msdn.microsoft.com/en-us/library/ms730128.aspx">http://msdn.microsoft.com/en-us/library/ms730128.aspx</a> 
  </li>
  
  
  <li>
    Mehran Nikoo best practices<br />
        <br /><a title="http://mehranikoo.net/CS/archive/2008/05/31/WCF_5F00_Best_5F00_Practices.aspx" href="http://mehranikoo.net/CS/archive/2008/05/31/WCF_5F00_Best_5F00_Practices.aspx">http://mehranikoo.net/CS/archive/2008/05/31/WCF_5F00_Best_5F00_Practices.aspx</a> 
  </li>
  
  
  <li>
    Web service software factory for creating robust WCF applications<br />
        <br /><a title="http://msdn.microsoft.com/en-us/library/cc487895.aspx" href="http://msdn.microsoft.com/en-us/library/cc487895.aspx">http://msdn.microsoft.com/en-us/library/cc487895.aspx</a> 
  </li>
  
</ul>


<p>
  Enjoy this tutorial and stay tuned for the next one!
</p>


<p>
  Tags: <a href="http://technorati.com/tag/WCF" rel="tag">WCF</a> <a href="http://technorati.com/tag/Prism" rel="tag">Prism</a> <a href="http://technorati.com/tag/WPF" rel="tag">WPF</a> <a href="http://technorati.com/tag/Composite application" rel="tag">Composite application</a>
</p>