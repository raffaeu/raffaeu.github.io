---
id: 549
title: Do you Test your applications?
date: 2013-08-25T15:47:00+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=549
permalink: /archive/:year/:month/:day/:title/
categories:
  - Agile
  - Design Pattern
  - TDD
---
When I talk about _test_ with a collegue, a tech friend or anybody involved in _Software Development,_ the conversation always ends up with a comparison between _Unit Tests_ and _Integration Tests_. As far as I am aware of, I know four type of developers, the one that practices unit tests, the once that practices integration tests, the one that practices both and the one that doesn’t practice any test at all …

In my opinion just the fact that we _compare_ the two techniques means that we don’t really apply the concept of _test_ in our applications. Why? To make it more clear, let’s have a look at what is a _Unit Test_ and what is an _Integration Test_, but also what is a _Validation Test_.

> **Unit Test   
>** From [Wikipedia](http://en.wikipedia.org/wiki/Unit_testing): A unit test is a test for a specific unit of source code, it is in charge of proving that the specific unit of source code is <u>fit for the use</u>.

> **Integration Test   
>** From [Wikipedia](http://en.wikipedia.org/wiki/Integration_testing): An integration test is a test that proves that multiple modules, covered by unit tests, can work together in integration.

> **Validation Test   
>** From [Wikipedia](http://en.wikipedia.org/wiki/Verification_and_Validation_(software)): A validation test is a test that proves that the entire application is able to satisfy the requirements and specifications provided by the entity who requested the application (customer).

We have three different type of tests for an application. They are quite different and indipendent from each other and in my opinion you can’t say an application “_is working”_ only because you achieved 100% code coverage or because you have a nice set of [Behavioral tests](http://blog.raffaeu.com/archive/2013/07/29/bdd-frameworks-for-net.aspx). You can say that your application is <u>tested</u> if:

> The modules are created using TDD so that we know the code is 100% covered, then those modules are tested to work together in a “__real environment_”_, finally we should also verify that our result is the one expected by the customer.

## An architecture to be tested

As a Software Architect I need to be sure that the entire architecture I am in charge of, is tested. This means that somehow, depending on the team/technology I am working with, I need to find a solution to test (unit, integration and validation) the final result and **<u>feel confident</u>**.

> What do I mean with _feel confident_? I mean, I should be confident when the build is **green** that I can go to my Product Owner or Product Manager and says _“Hey, a new release is available!”_. 

So, as usual for my posts, we need a sample architecture to continue the conversation.

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; border-bottom-width: 0px; display: inline; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/08/image_thumb6.png" width="260" height="222" />](http://blog.raffaeu.com/wp-content/uploads/2013/08/image6.png)&#160; 

Here we have a classic [multi-layer architecture](http://en.wikipedia.org/wiki/Multitier_architecture), where we have the data, the data acces, the business logic and the presentation layer, logically separated. We can consider each layer a separate set of modules that need to speak to each other.

<u>How do we test this? </u>   
In this case I use a specific pattern that I developed during the years. First, each piece of code requires a **unit test**, so that I can achieve the first test requirement, the <u>unit test requirement</u>. Then I need to create a test **environment** where I can run multiple modules together, to test the <u>integration</u> between them. When I am satisfied with these two typologies of tests, I need a **QA** that will verify the final result and achieve also the <u>validation test</u>.

Below I have just created an example of the different type of tests that I may come up with in order to be able to test the previosuly described architecture.

## Unit Tests

First of all, the **unit test**. The unit test should be spreaded everywhere and achieve always the [100% code coverage](http://en.wikipedia.org/wiki/Code_coverage). Why? Very simple, the concept behind TDD is to _“write a test before write or modify a piece of code”_; so if you code in this way, you will always have 100% code coverage … In the same way if you don’t achieve 100% code coverage, <u>you are probably doing something wrong</u> …

Considering now our data layer, for example, I should have the following structure into my application. A module per context with a correspondent test module :

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; border-bottom-width: 0px; display: inline; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/08/image_thumb7.png" width="190" height="258" />](http://blog.raffaeu.com/wp-content/uploads/2013/08/image7.png)&#160;

This is enough to achieve 100% code coverage and comply to the <u>first test category</u>:<u> </u>**unit test**. </p> 

## Integration Tests

Now that the functionalities are implemented and covered by a bunch of unit tests, I need to design and implement a _[test environment](http://technet.microsoft.com/en-us/library/cc750093.aspx)_, an environment where I can deploy my code and test it in integration mode.

The schema below shows how I achieve this using Microsoft NET and the Windows platform. I am quite sure you can achieve the same result on a Unix/Linux platform and on other systems too.

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; border-bottom-width: 0px; display: inline; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/08/image_thumb8.png" width="520" height="328" />](http://blog.raffaeu.com/wp-content/uploads/2013/08/image8.png) 

In my case this is the sequence of actions triggered by a check-in:

  * The code is sent to a repository (TFS, Git, …) 
  * The code is build and: 
      * Unit tested 
      * Code covered analysis 
      * Code quality and style analysis 
  * Then the code is deployed to a test machine 
      * The database is deployed for test 
      * The web tier is deployed for test 
      * The integration tests are executed and analyzed 

This set of actions can make my build green, red or partial green. From here I can decide if the code needs additional reviews or it can be deployed to a staging environment. This type of approach is able to prove that the **integration** between my modules is working properly. 

## Finally, validate everything with someone else

Now, if I got lucky enough I should have a green build, that in my specific case it is deployed in automation to a repository available on the cloud. In my specific case I use [TFS Preview service](http://tfs.visualstudio.com/) and for the virtual machines I am using [Azure Virtual Machines](http://www.windowsazure.com/en-us/services/virtual-machines/). Everything is deployed using some automation tools like [Octopus Deploy](http://octopusdeploy.com/), which allows me to automatically scripts all the steps required for my integration tests and for my deployment steps.

The same tool is used by the Product Owner to automate the QA, Staging and Production deployment of the final product. The interface allows you to select the version and the environment that needs to be upgraded or downgraded:

<img src="http://i.octopusdeploy.com/home/dashboard-big.png" width="450" height="187" />

I can choose to “go live”, “go live and test” and many more options, without the needs to manually interact with the deployment/test process.

So, if you are still following me, the real concept of _test_ is expressed only when you can easily achieve all the three phases of the test process: unit, integration and validation. The picture below represents the idea:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; border-bottom-width: 0px; display: inline; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/08/image_thumb9.png" width="470" height="166" />](http://blog.raffaeu.com/wp-content/uploads/2013/08/image9.png) </p> 

Hope it makes sense.