---
id: 641
title: Configure MTM 2013 to run automated tests
date: 2014-04-30T22:40:00+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=641
permalink: /archive/:year/:month/:day/:title/
categories:
  - Agile
  - TFS
tags:
  - MTM
  - Octopus
  - TFS
---
### The Scenario

I have an MTM 2013 installation that is configured in the following way:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_thumb.png" width="470" height="346" />](http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image.png)

This is the workflow that is triggered when a Developer check-in something:

  1. The code is built by TFS 2013, using a TFS Build Agent 
  2. The agent update a <a href="http://www.nuget.org/" target="_blank">Nuget</a> Package containing the deployed application 
  3. Octopus release the package over our Staging environment 
  4. MTM execute remote tests after the Build is complete 

### Configuring MTM 2013

In order to have a successful and pleasant experience with MTM 2013 we need to pre-configure in the proper way the test environment(s). If you don’t configure properly the Test machines, the Environments and/or the Test Cases you will have a lot of troubleshooting activities in your backlog … MTM is quite articulated.

The time I am writing this article is April 2014 and MTM came out a while ago, so after you install it you may face some missing values in the operating systems or in the browsers list. So, first of all, let’s update these value lists.

Open MTM and Choose **Testing Center>Test Configuration Manager>Manage configuration variables**. In my case I extended the values in the following way:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_thumb_3.png" width="470" height="314" />](http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_3.png)

You can also go directly to the source and change the XML entries. In order to change the correct file I would suggest you to visit this useful MSDN page:   
[http://msdn.microsoft.com/en-us/library/ms243856.aspx](http://msdn.microsoft.com/en-us/library/ms243856.aspx "http://msdn.microsoft.com/en-us/library/ms243856.aspx")

Now that I have my value lists updated I can start with the configuration process. I have highlighted below the steps you should follow in order to have a proper MTM configuration.

  1. Define the Environment   
    [http://msdn.microsoft.com/en-us/library/ee943321(v=vs.110).aspx](http://msdn.microsoft.com/en-us/library/ee943321(v=vs.110).aspx "http://msdn.microsoft.com/en-us/library/ee943321(v=vs.110).aspx") 
  2. Define the Test Configurations   
    [http://msdn.microsoft.com/en-us/library/dd286643.aspx](http://msdn.microsoft.com/en-us/library/dd286643.aspx "http://msdn.microsoft.com/en-us/library/dd286643.aspx") 
  3. Create or Import the Test Cases   
    [http://msdn.microsoft.com/en-us/library/dd380741.aspx](http://msdn.microsoft.com/en-us/library/dd380741.aspx "http://msdn.microsoft.com/en-us/library/dd380741.aspx") 
  4. Create a Test Plan for your backlog   
    [http://msdn.microsoft.com/en-us/library/dd380763.aspx](http://msdn.microsoft.com/en-us/library/dd380763.aspx "http://msdn.microsoft.com/en-us/library/dd380763.aspx") 
  5. Execute a Test Automation and Configure it   
    [http://msdn.microsoft.com/en-us/library/ee257067(v=vs.100).aspx](http://msdn.microsoft.com/en-us/library/ee257067(v=vs.100).aspx "http://msdn.microsoft.com/en-us/library/ee257067(v=vs.100).aspx") 
  6. Trigger automated tests after a build complete 

Let’s have a look at each of these steps, or you can follow the MSDN link I have attached to each one of them.

### #01 – Define your Environment

First of all you need to install an MTM Controller. Usually I install it on the same location of my main TFS 2013 instance (not the build servers …). After I have installed the Controller I can start to register my agents. 

For the controller and agent installations and configuration follow this link:   
[http://msdn.microsoft.com/en-us/library/hh546459.aspx](http://msdn.microsoft.com/en-us/library/hh546459.aspx#agent "http://msdn.microsoft.com/en-us/library/hh546459.aspx#agent")

**Note:** if you don’t have any agent registered in your Controller you will not be able to configure the environments. In my case I try to keep the Machines’ classification identical between Build, Deployment and Test tools. So, in my case, I have the following structure:

_Staging > Production > Cloud_

And this is the expected result in my MTM configuration. 

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_thumb_4.png" width="470" height="174" />](http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_4.png)

After you install a new Agent remember to refresh the Dashboard. Also, if you are facing troubles registering the Agent, try to reboot the Controller and the Agent machines, sometimes it helped me to move forward with the registration.

And in my environment overview dashboard

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_thumb_5.png" width="340" height="180" />](http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_5.png)

**One final note here** _if you choose to have an “external” virtualization mechanism and work without SCVMM you will not have access to some functionalities like reboot, clone and manage environments because they are not handled by SCVMM_. ie if you are using VMWare

### #02 – Create some configurations

Configurations are used by MTM to define different _test environment scenario_. Let’s assume that your MTM is testing a WPF Client Application, probably you want to know how it runs over multiple Operating Systems. For this and many other reasons, you can create inside MTM multiple configurations to test your application over multiple environments, operating systems, browsers and/or SQL Server instances.

The picture below show some of the configurations I use while testing a WPF Client application. I use different operating systems, different languages and different browsers to download the ClickOnce application. It should work exactly the same over all these configurations.

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_thumb_6.png" width="470" height="226" />](http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_6.png)

When I am done with this part, _before assigning test plan to configurations and machines to configurations_, I need to complete the setup of my set harness.

### #03 – Create or import the Test Cases

After you are done with the configuration of MTM it’s time to prepare our backlog in order to be able to manage the tests execution. MTM requires that your tests are identified by a **test case** work item. In order to do that you have two options:

  * Manually create your test cases and associated them with an automation if you need to automate it, or create a manual test and register it within your backlog in TFS 
  * Import your automation from an **MsTest** class library, using the tcm command:&#160;  
  
    <pre class="csharpcode">tcm testcase 
  /collection: CollectionUrl 
  /teamproject:MyProject 
  /import 
  /storage:MyAssembly.dll 
  /category:"MyIntegrationTestCategory"</pre>

and at the end you will have your test cases created automatically for you like the following screenshot shows:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_thumb_7.png" width="370" height="131" />](http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_7.png)

Now open MTM and go to **Testing Center > Track > Queries** and you can start to search for your test cases. In this phase you’ll notice how important is to keep a good and constant naming convention for your tests and to work with categories:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_thumb_8.png" width="470" height="202" />](http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_8.png)

Why? Because with a proper naming convention you can create a query and group your work items in an easier way

### #04 – Create a Test Plan

There are multiple ways of creating a test plan. You can create a test plan manually and then add a test case, one by one. This is quite useful if you are working on a new project and sprint by sprint you simply add the test cases as soon as you create them.

Another option, _which I personally love (ndr)_, is to create a Test Suite composed by multiple Test Cases, generated by a query. Why is this very useful? Well first of all you don’t have to touch anymore because every time you add a new test case it will just be included in the Test Suite. Second, it will **force you and your team** _to use a proper Test Naming Convention_.

In my case, I know the Area of my tests, but I want to test only the PostSharp aspects, nothing else, so I can write a query like the following:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_thumb_9.png" width="470" height="388" />](http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_9.png)

and associate the Suite query generated with a parent one, like I did in my projects. After a while you will end up with a series of test suite (_test harness_) grouped by a certain logic. For example you can have test suites generated by a DSL expression or by a test requirement created by a PO or a QA:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_thumb_10.png" width="470" height="161" />](http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_10.png)

### #05 – Run your Automation

Before running the automation you need to inform MTM about few things. If you think about it for a moment, when you execute local tests you usually have a _test settings file_ which is used to inform **MsTest** about the assemblies that need to be loaded, plugins and other test requirements.

Inside MTM, you can inspect the settings by opening the test plan properties window.
    
  
Within this windows you can choose settings for a Local run but also for a Remote run. In my case, when I run a remote test I need to be sure that a specific file is deployed, so this is what I have done in my configuration:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_thumb_11.png" width="470" height="226" />](http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_11.png)

And when I manually trigger a Test I just ensure that the right configuration is picked up, like here:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_thumb_12.png" width="370" height="217" />](http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_12.png)

and that’s it. Now you know how to prepare MTM for automation, how to configure it and how to group and manage test suites. With this configuration in place you should be able to trigger tests in automation after a build is complete.

Last piece of the puzzle could be “_how do I trigger those tests after my build is complete?_” and here we come with the latest part of this tutorial.

### #06 – Trigger automated tests after a build complete

With TFS 2013 we got a new Workflow Template called **LabDefault** template. In order to use it you have to create a new build and select this Template. 

After you have setup the new build you can go in the Process tab and specify how you want to execute your automated tests.

For example, you can choose which environment will be used for your test harness:
    
  
[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_thumb_13.png" width="470" height="121" />](http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_13.png)

Which Build output will be used for the tests. You can either trigger a new build or get the assemblies from the **latest successful build** or even trigger a new customized workflow on fly: 

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_thumb_14.png" width="470" height="268" />](http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_14.png)

And what **Test Plan** you want to execute, where and how: 

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_thumb_15.png" width="470" height="202" />](http://blog.raffaeu.com/wp-content/uploads/Configure-MTM-2013-to-run-automated-test_B9BD/image_15.png)

### Conclusion

I hope you will find this post useful cause for me the configuration of MTM took a while and I truly struggled to find a decent but short post that highlights the steps that need to be done in order to have MTM working properly.