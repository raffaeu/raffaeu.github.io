---
id: 496
title: CQRS in brief
date: 2013-07-14T14:54:00+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=496
permalink: /archive/2013/07/14/cqrs-in-brief.aspx
categories:
  - Design Pattern
tags:
  - CQRS
---
<img style="float: left; display: inline" align="left" src="http://logofury.com/wp-content/uploads/2004/eske_CQRS.gif" width="240" height="192" />Around the web there is a lot of noise about “CQRS” and “Event sourcing”. Almost everybody involved in a layered application is now trying to see if CQRS can fit in its current platform.

I also found some nice tutorial series about CQRS and some good explanation of event sourcing but what I didn’t see yet is a nice architectural overview of these two techniques and when they are needed, what are the pros and the cons.

So let&#8217;s start as usual on my blog with a brief introduction to CQRS. In the next post I will show Event Sourcing.

### CQRS, when and who?

Around the end of 2010, <a href="http://codebetter.com/gregyoung/" target="_blank">Greg Young</a>, wrote a document about Command Query Responsibility Separation, available at <a href="http://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf" target="_blank">this link</a>. The documentation highlights the advantages of this pattern, the different concepts of command UX (_task based UX_), the CQRS pattern and the event sourcing mechanism. So, if you want to read the real original version of this architectural pattern you need to refer to the previous mentioned doc.

In the meantime also <a href="http://martinfowler.com/" target="_blank">M. Fowler</a> started to talk about it, with the post “<a href="http://martinfowler.com/bliki/CQRS.html" target="_blank">CQRS</a>”. Without forgetting the series of posts published by <a href="http://www.udidahan.com/" target="_blank">Udi Dahan</a> (the creator of <a href="http://particular.net/" target="_blank">NServiceBus</a>).

Last but not least, Microsoft <a href="http://msdn.microsoft.com/en-us/library/ff921345.aspx" target="_blank">patterns and practices</a> created a nice series related to CQRS called <a href="http://msdn.microsoft.com/en-us/library/jj554200.aspx" target="_blank">CQRS Journey</a>. It’s a full application with a companion book that can also be downloaded for free form MSDN. It shows what would really happen within your team when you will start to apply concepts like: bounded context, event sourcing, domain driven design and so on.

### CQRS in brief

In brief CQRS says that we should have a layer in charge of handling the **data requests (queries)** and a layer in charge of handling the **command requests (modification)**. The two layers should not be aware of each other and should/may return different objects, or better, the query layers should return serializable objects containing information that need to be read, while the command layer should receive serializable objects (commands) that contains _<a href="http://cqrs.wordpress.com/documents/task-based-ui/" target="_blank">the intention of the user</a>_.

Why? Because during the lifecycle of an application is quite common that the Logical Model become more sophisticated and structured, but this change may impact also the UX, which should be independent from the core system.   
In order to have a scalable and easy to maintain application we need to reduce the constraints between the reading model and the writing model.

The picture below show a brief explanation of how a CQRS application should work (_this is just one of the possible architecture and it’s not the silver bullet solution for CQRS_):

**Read Mechanism**

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/07/image_thumb.png" width="660" height="219" />](http://blog.raffaeu.com/wp-content/uploads/2013/07/image.png)

We will use REST API to query the data because they provide a **standard** way of communication between different platforms. In our case the REST API will host something like the following API URLs:

<table cellspacing="0" cellpadding="2" width="400" border="0">
  <tr>
    <td valign="top" width="94">
      <strong>Get Person</strong>
    </td>
    
    <td valign="top" width="108">
      <em>api/Person/{id}</em>
    </td>
    
    <td valign="top" width="197">
      <em>api/Person/10</em>
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="94">
      <strong>Get Persons</strong>
    </td>
    
    <td valign="top" width="108">
      <em>api/Person</em>
    </td>
    
    <td valign="top" width="197">
      <em>api/Person?Name eq ’Raf’</em>
    </td>
  </tr>
</table>

These APIs don’t offer Write or Delete support because it will break the CQRS principles. They return single or collection of serializable objects (DTO), result of a query sent previously by the user.

**Write Mechanism**

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/07/image_thumb1.png" width="660" height="257" />](http://blog.raffaeu.com/wp-content/uploads/2013/07/image1.png)

For the write model we can use WCF, send a command message (<a href="http://www.enterpriseintegrationpatterns.com/CommandMessage.html" target="_blank">see pattern here</a>) and handle the command on our server side. The command will change the domain model and trigger a new message in the bus, a message that **acknowledge everybody** about the status changed. It will also update the write datastore with the new information. 

What’s important is that the command is not anymore a CRUD operation but the _intent of the user_ to act against the model.

### How does it work?

The first question I try to answer as soon as I start to read about a new architectural pattern like CQRS is, “yes nice but how it is supposed to work?”. Usually I don’t want to read code to understand that, but I would like to see a nice diagram (UML or similar) that shows me the flow of the application through the layers, so that I can easily understand how it is supposed to be done.

I tried to apply this concept to CQRS and again, I split the diagram into read and write.

**The read part**

  * A User ask some data to the view 
      * The view send an **async** request to the REST API 
          * The REST API query the datastore and returns a result 
              * The result is returned by the callback in the view </ul> 
              * The User will see the data requested when the view refresh</ul> 
            [<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/07/image_thumb2.png" width="660" height="270" />](http://blog.raffaeu.com/wp-content/uploads/2013/07/image2.png)
            
            **The write part**
            
              * A User send a command from the UI (save, change, order …) _containing the intention of the user  
_ Usually asynchronous 
                  * The command is sent using JSON to a WCF endpoint (_one of the 1,000 possible solutions to send a serializable command_) and return an **acknowledgment**
                  * The WCF endpoint has a command handler that will execute the command 
                      * The command changes the domain which 
                      * Raise an event 
                          * Save it’s state on the database</ul> </ul> 
                          * The service bus receive the events and **acknowledge** the environment  
                            (_for instance, here you can have event sourcing that listen to the event_) 
                              * The user interface is updated by the events and the acknowledgments</ul> 
                            [<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/07/image_thumb3.png" width="660" height="269" />](http://blog.raffaeu.com/wp-content/uploads/2013/07/image3.png)
                            
                            As you can see, Domain Model, Event Sourcing, Data Mapping and Service Bus are optional and additional architectural patterns that can be merged with CQRS, but not part of it.
                            
                            With CQRS your primary concern is to loosely couple the **reads** from the **writes**.
                            
                            ### CQRS Pros and Cons
                            
                            Ok, this architectural pattern (there are different opinions here …) it’s quite helpful but there are some cons that we should keep in consideration when we want to adopt CQRS.
                            
                            When **not to use it?**
                            
                              * You have a simple (no complex logic) application that executes CRUD, in this case you can easily map the domain with an ORM an provide DTO back/forward to your client tier. Even the UX can reflect the CRUD style. 
                                  * Your concern is not about performances and scalability. If your application will not be on a Saas platform, but only internal to a company, probably CQRS will over engineer the whole architecture, especially if the user interface is complex and data-entry based. 
                                      * You can’t create a task based UI and your customer doesn’t have yet the culture for a custom task based application.</ul> 
                                    Also, remember that CQRS is just a portion of your architecture, the one that take care to expose data and/or receive commands. You still have to deal with the data layer, your domain or your workflows, your client logic and validation and more.
                                    
                                    If you have already analysed those concepts and you feel confident about your architecture, then you may consider to see how it will fit CQRS inside it, or you can simply start by splitting the reading endpoints of your API from your writing endpoints. Especially because right now the primary requirement for a modern application is to expose data through a simple and standardize channel like REST, so that the integration with external platform it’s easier and more standardized.
                                    
                                    ### Alternative?
                                    
                                    There are a couple of alternatives that we can keep in consideration if we don’t want to face the complexity of a CQRS solution. It really depends on what type of User Experience we need to provide.
                                    
                                    **Data entry application**
                                    
                                    In this case it’s quite easy to keep going with a solution similar to the classic CRUD applications, maybe a sort of master/details approach where a View is mapped to a Domain Entity which is bind to the database using an ORM.
                                    
                                    This is a classic, very efficient and productive way of creating a CURD application with the unnecessary over engineering SOA on top of it. We don’t need DTO and probably we will just have a data-layer that takes care of everything.
                                    
                                    [<img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/07/image_thumb4.png" width="660" height="355" />](http://blog.raffaeu.com/wp-content/uploads/2013/07/image4.png)
                                    
                                    **SOA platform or API public**
                                    
                                    Sometimes we may need to provide access to the system through a public API, at the moment REST seems the most feasible and globalized way. In this case we may still have a Domain and an ORM behind the hood but we can’t directly expose those to the web. So? So we need a DTO and a communication channel where we can expose those simple objects in order to send and receive data in a standardized way.
                                    
                                    In this case we are starting to face more complexity due to the SOA constraints:
                                    
                                      * Reuse, interoperability and modularity 
                                          * Standard compliance 
                                              * Service identification, classification and categorization 
                                                  * more …</ul> 
                                                [<img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/07/image_thumb5.png" width="660" height="401" />](http://blog.raffaeu.com/wp-content/uploads/2013/07/image5.png)
                                                
                                                In this case we can have a client app that works using ViewModels bind to the DTO exposed by our Web API, but we are still not using CQRS at all because we don’t distinguish between data and intention of the user.
                                                
                                                > I hope that my two notes can give you a bit more clear picture of what is CQRS and what is supposed to be from my point of view, of course! <img class="wlEmoticon wlEmoticon-smile" style="border-top-style: none; border-left-style: none; border-bottom-style: none; border-right-style: none" alt="Smile" src="http://blog.raffaeu.com/wp-content/uploads/2013/07/wlEmoticon-smile.png" />