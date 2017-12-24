---
id: 436
title: Castle WCF Facility
date: 2013-04-05T09:42:00+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=436
permalink: /archive/2013/04/05/castle-wcf-facility.aspx
categories:
  - 'C#'
  - Design Pattern
  - WCF
---
In the <a href="http://blog.raffaeu.com/archive/2013/01/31/wcf-and-dependency-inversion-using-castle-windsor.aspx" target="_blank">previous post</a> we saw how to provide _Inversion of Control_ capabilities to a WCF service. That’s pretty cool but unfortunately it requires a big refactoring if you plan to apply the Dependency Inversion pattern on an existing WCF project. 

Today we will see an interesting alternative provided by Castle project. If you are not aware of Castle, you can have a look at the community web site <a href="http://www.castleproject.org/" target="_blank">here</a>. Castle provides a set of tools for NET, _Active Record, Dynamic Proxy, Windsor_ and a set of **facilities** that you can easily plug into your code.

## The demo project

Before starting to have&nbsp; a look at **Castle Wcf Facility** we need a new WCF project and a WCF web site to host our service. The idea is to create an empty service and host it, then we will refactor the project to include Castle Windsor and WCF facility.

Let’s start by creating an IIS web site, a DNS redirect and a folder for our IIS web site. The final result will be like the following one:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/04/image_thumb.png" width="351" height="79" />](http://blog.raffaeu.com/wp-content/uploads/2013/04/image.png)

Now we need to create the new project in Visual Studio. I have created a solution that contains 2 projects:

  * Empty WCF Service Library named “_Service.Contracts_” 
      * Empty WCF Service Library named “_Service.Implementations_” 
          * reference the project “_Service.Contracts_”</ul> 
    And this would be the final result of this step:
    
    [<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/04/image_thumb1.png" width="202" height="191" />](http://blog.raffaeu.com/wp-content/uploads/2013/04/image1.png)
    
    > We have just implemented the _correct_ way of creating a WCF service implementation and a WCF service contract. The Client will have a reference to the _contract_ assembly in order to be able to work with the service implementation, _without the need of using a Proxy_. This concept is outside the fact that I will use an ioc container, it’s just a lot easier than adding a “web service reference” in visual studio and carry tons of useless proxy classes in your project.
    
    ## Hosting WCF service
    
    The final step is to host this WCF Service inside an IIS ASP.NET or ASP.NET MVC website. I have create a new “_web site application_” and from the options I choose “_WCF service application_”. Then I select my local IIS for the development path, but this is up to you.
    
    [<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/04/image_thumb2.png" width="260" height="130" />](http://blog.raffaeu.com/wp-content/uploads/2013/04/image2.png)
    
    Now Visual Studio will ask you if you want to override the current web site or use the existing content. If you choose the second option you will have to provide also web.config configuration and initial setup, so I kindly suggest the first option.
    
    If everything went ok you should be able to visit the default service created by Visual Studio at this url: <http://staging.service.local/Service.svc>. 
    
    Now we need to change the content and the name of the .svc file, but before we need to add a reference for _Contracts_ and _Implementations_ from the solution explorer!
    
    <div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:be9b3b93-539f-401d-9877-0a7df1c02419" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px">
      <pre style=white-space:normal> [sourcecode language='php' ] <%@ ServiceHost Language="C#" Debug="true" Service="Service.Implementations.ServiceImplementation" %> [/sourcecode] </pre>
    </div>
    
    We need to reference the Implementation because we don’t have a factory yet able to translate the _IServiceContract_ into a _ServiceImplementation_ class. So the next step is to install in our solution the WCF castle facility nuget package.
    
    [<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/04/image_thumb3.png" width="299" height="147" />](http://blog.raffaeu.com/wp-content/uploads/2013/04/image3.png)
    
    ## Add WCF facility
    
    So far we didn’t need any external tool because we have an empty service created by default by Visual Studio with a dummy code, or probably during the tutorial you have already changed your service to so something more interesting. The next step is to add a facility in order to be able to change the default factory used by IIS to create an instance of our service. 
    
    The first step is to add a new _global.asax_ file if you don’t have one already and plug the Windsor container inside the _Application_Start_ event.  
    This is my global.asax file:
    
    <div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:5a1d79c1-5588-446a-a279-52d2a994f8ca" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px">
      <pre style=white-space:normal> [sourcecode language='php' ] <%@ Application Language="C#" Inherits="ServiceApplication" %> [/sourcecode] </pre>
    </div>
    
    Now, in the code behind file we want to create an instance of a new Windsor Container during the _Application_Start_ event:
    
    <div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:306dfa3b-7490-4833-84b9-cb1d1cd3e4ce" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px">
      <pre style=white-space:normal> [sourcecode language='csharp' ] public class ServiceApplication : HttpApplication { public static WindsorContainer Container { get; set; } protected void Application_Start() { BuildContainer(); } } [/sourcecode] </pre>
    </div>
    
    And this is the code to initialize a new container. In this code we need to pay attention in the way we specify the server name, because WCF facility uses the **service** attribute value to search inside the Windsor container. So, this is the code to register the service:
    
    <div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:7aad7b42-9271-44c1-862e-89319a0b0d39" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px">
      <pre style=white-space:normal> [sourcecode language='csharp' ] private void BuildContainer() { Container = new WindsorContainer(); Container.Kernel.AddFacility<WcfFacility>(); Container.Kernel.Register(Component.For<IServiceContract>() .ImplementedBy<ServiceImplementation>() .Named("ServiceContract")); } [/sourcecode] </pre>
    </div>
    
    And this is the change we need to apply to the Service.svc file:
    
    <div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:33b6f515-f186-4ff8-a116-eee814580900" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px">
      <pre style=white-space:normal> [sourcecode language='php' ] <%@ ServiceHost Language="C#" Debug="true" Service="ServiceContract" Factory="Castle.Facilities.WcfIntegration.DefaultServiceHostFactory, Castle.Facilities.WcfIntegration" %> [/sourcecode] </pre>
    </div>
    
    Now your service is ready to use dependency injection under the power of Castle Windsor. 
    
    ## Add a dependency to a WCF service using Windsor
    
    Simple example, I have a service that requires an _ILogger_ injected in it. I need to change the code for my service to allow injection and then I need to register the logger in the global.asax file during the Windsor container activation process.
    
    <div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:77bf6284-8f3b-4ff0-aca9-9a92f6b07862" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px">
      <pre style=white-space:normal> [sourcecode language='csharp' ] public class ServiceImplementation : IServiceContract { private readonly ILogger logger; public ServiceImplementation(ILogger logger) { this.logger = logger; } public string GetData(int value) { this.logger.Trace("Method started!"); return string.Format("You entered: {0}", value); } } [/sourcecode] </pre>
    </div>
    
    And this how we change the registration in the container:
    
    <div id="scid:C89E2BDB-ADD3-4f7a-9810-1B7EACF446C1:eddf5f67-d1a0-43d1-bd54-c507e317f8ad" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px">
      <pre style=white-space:normal> [sourcecode language='csharp' ] Container.Kernel.Register( // register the logger Component.For<ILogger>() .ImplementedBy<TraceLogger>(), // register the service Component.For<IServiceContract>() .ImplementedBy<ServiceImplementation>() .Named("ServiceContract")); [/sourcecode] </pre>
    </div>
    
    The source code is available here: 
    
    [http://sdrv.ms/16BLYoI](http://sdrv.ms/16BLYoI "http://sdrv.ms/16BLYoI")