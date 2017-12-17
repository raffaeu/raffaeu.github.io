---
id: 533
title: Domain Model in Brief
date: 2013-08-18T19:50:00+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=533
permalink: /archive/:year/:month/:day/:title/
categories:
  - Agile
  - 'C#'
  - Design Pattern
  - TDD
---
[This series](http://blog.raffaeu.com/archive/2013/07/14/cqrs-in-brief.aspx) is getting quite interesting so I decided to post something about [Domain Model](http://en.wikipedia.org/wiki/Domain_model). Just a little introduction to understand what is and what is not Domain Model.

Honestly this pattern is often misunderstood. I see tons of examples of anemic models that reflect 1:1 the database structure, mirroring the classic [Active Record](https://en.wikipedia.org/wiki/Active_record_pattern) pattern. But Domain Model is more than that …

Let’s start as usual with a definition, and we take this definition from one of the founder of the Domain Model pattern, Martin Fowler:

> An object model of the domain that incorporates both behavior and data.

But unfortunately I almost never see the first part (_behaviors_). It’s easy to create an object graph, completely anemic, and then hydrate the graph using an ORM that retrieves the data from the database; but if we skip the logical part of the object graph, we are still far from having a correct Domain Model.

This means that <u>we can have anemic objects</u> that represent our data structure, but we can’t say that we are using Domain Model.

## Our Domain Model

In order to better understand the Domain Model pattern we need a sample Domain, something that aggregates together multiple objects with a common scope.

In my case, I have an Order Tracking System story that I want to implement using the Domain Model pattern. My story has a main requirement:

> As a _Customer_ I want to register my _Order Number_ and _e-mail_, in order to receive updates about the _Status_ of my Order

So, let’s draw few acceptance criteria to register an Order:

  * An Order can be **created** only by a user of type **Administrator** and should have a valid Order id 
  * An Order can be **changed** only by a user of type **Administrator** 
  * Any user can **query** the **status** of an Order 
  * Any user can **subscribe** and receive updates about Order Status changes using an e-mail address 

If we want to represent this Epic using a Use Case diagram, we will probably end up with something like this:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; border-bottom-width: 0px; display: inline; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/08/image_thumb.png" width="520" height="238" />](http://blog.raffaeu.com/wp-content/uploads/2013/08/image.png)

Great, now that we have the specifications, we can open Visual Studio (_well I did it previously when I created the diagram …_) and start to code. Or better, start to make more investigations about our Domain objects.

It’s always better to have an iteration 0 in DDD when you start to discover the Domain and the requirements together with your team. I usually discover my Domain using mockups like the following one, where I share ideas and concepts in a _fancy way._

[<img title="Snapshot" style="border-top: 0px; border-right: 0px; border-bottom: 0px; border-left: 0px; display: inline" border="0" alt="Snapshot" src="http://blog.raffaeu.com/wp-content/uploads/2013/08/Snapshot_thumb.png" width="520" height="231" />](http://blog.raffaeu.com/wp-content/uploads/2013/08/Snapshot.png) 

## Create a new Order

An _Agent_ can created an _Order_ and the Order should have a proper _Order Id_. The order id should reflect some business rules so it’s ok to have this piece of validation logic inside our domain object. We can also say that the Order Id is a **requirement** for the Order object because we can’t create an Order object without passing a valid Order Id. So, it makes absolutely sense to _encapsulate_ this concept into the Order entity.

A simple test to cover this scenario would be something like this:

<pre class="csharpcode">[Fact]
<span class="kwrd">public</span> <span class="kwrd">void</span> OrderInitialStateIsCreated()
{
    <span class="kwrd">this</span>
        .Given(s =&gt; s.GivenThatAnOrderIdIsAvailable())
        .When(s =&gt; s.WhenCreateANewOrder())
        .Then(s =&gt; s.ThenTheOrderShouldNotBeNull())
            .And(s =&gt; s.TheOrderStateShouldBe(OrderState.Created))
        .BDDfy(<span class="str">"Set Order initial Status"</span>);
}</pre>

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; border-bottom-width: 0px; display: inline; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/08/image_thumb1.png" width="320" height="144" />](http://blog.raffaeu.com/wp-content/uploads/2013/08/image1.png)

> _If you don’t know what [Fact] is, it is used by_ [_xUnit_](http://xunit.codeplex.com/)_, an alternative test framework that can run within Visual Studio 2013 test runner using the_ [_available nuget test runner_](http://xunit.codeplex.com/discussions/260634) _extension package._

From now on, we have our first Domain Entity that represents the concept of _Order_. This entity will be my Aggregate root, an entity that bounds together multiple objects of my Domain, in charge of guarantee consistency of changes made to those objects.

## Domain Events

M. Fowler [defines a Domain event](http://martinfowler.com/eaaDev/DomainEvent.html) in the following way:

> A Domain event is an event that captures things in charge of changing the state of your application

Now, if we change the _Order Status_ we want to be sure that an event is fired by the Order object, which inform us about the Status change. _Of course this event will not be triggered when we create a new Order object_. The event should contains the Order Id and the new Status. In this way we will have the **key information** for our domain object and we _may not be required to repopulate the object from the database._

<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">void</span> SetState(OrderState orderState)
{
    state = orderState;
    <span class="kwrd">if</span>(OrderStateChanged != <span class="kwrd">null</span>)
    {
        OrderStateChanged(<span class="kwrd">new</span> OrderStateChangedArgs(<span class="kwrd">this</span>.orderId, <span class="kwrd">this</span>.state));
    }
}</pre>

Using the Domain Event I can easily track the changes that affect my Order Status and rebuild the status in a specific point in time (_I may be required to investigate the order_). In the same time I can easily verify the current status of my Order by retrieving the latest event triggered by the Order object.

## The ubiquitous language

With my Order object create, I need now to verify that the behaviors assigned to it are correct, and the only way to verify that is to contact a Domain expert, somebody expert in the field of Order Tracking System. Probably witht this person I will have to speak a common language, the _[ubiquitous language](http://martinfowler.com/bliki/UbiquitousLanguage.html)_ mentioned by Eric Evans.

With this language in place, I can easily write some Domain specifications that guarantee the correctness of the Domain logic, and let the Domain Expert verifies it. 

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; border-bottom-width: 0px; display: inline; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/08/image_thumb2.png" width="370" height="312" />](http://blog.raffaeu.com/wp-content/uploads/2013/08/image2.png)

This very verbose test, is still a unit test and it runs in memory. So I can easily introduce BDD into my application without the requirement of having an heavy test fitness behind my code. BDDfy allows me to produce also a nice documentation for the QA, in order to analyze the logical paths required by the acceptance criteria.

Plus, I am not working anymore with _mechanical code_ but I am building my own language that I can share with my team and with the analysts.

## Only an Administrator can Create and Change an Order

Second requirement, now that we know how to create an Order and what happen when somebody changes the Order Status, we can think of creating an object of type User and distinguish between a normal user and an administrator. We need to keep this concept again inside the Domain graph. Why? Well, very simple, because we choose to work with the Domain Model pattern, so we need to keep logic, behaviors and data within the same object graph.

So, the requirements are the following:

  * When we create an Order we need to know who you are 
  * When we change the Order status, we need to know who you are 

In Domain Driven Design we need to give **responsibility** of this action to somebody, that’s overall the logic that you have to apply when designing a Domain Model. <u>Identify the responsibility and the object in charge of it</u>.

In our case we can say to the Order object that, in order to be created, it requires an object of type User and verify that the user passed is of type Administrator. This is _one possible option_, another one _could be to_ involve an external domain service, but in my opinion is not a crime if the Order object is aware of the concept <u>being an administrator</u>.

So below are my refactored Behavior tests:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; border-bottom-width: 0px; display: inline; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/08/image_thumb3.png" width="420" height="338" />](http://blog.raffaeu.com/wp-content/uploads/2013/08/image3.png)

The pre-conditions raised in the previous tests:

  * order id is valid 
  * user not null 
  * user is an administrator 

are inside my Order object constructor, because in DDD and more precisely in my Use Case, it doesn’t make any sense to have an invalid Order object. So the Order object is <u>responsible to verify the data provided in the constructor is valid</u>.

<pre class="csharpcode"><span class="kwrd">private</span> Order(<span class="kwrd">string</span> orderId, User user)
{
    Condition
       .Requires(orderId)
       .Contains(<span class="str">"-"</span>,<span class="str">"The Order Id has invalid format"</span>);
    Condition
       .Requires(user)
       .IsNotNull(<span class="str">"The User is null"</span>);
    Condition
       .Requires(user.IsAdmin)
       .IsTrue(<span class="str">"The User is not Administrator"</span>);

    <span class="kwrd">this</span>.orderId = orderId;
}</pre>

> _**Note:** for my assertions I am using a nice project called_ [_Conditions_](http://conditions.codeplex.com/) _that allows you to write this syntax._ 

Every time I have an Order object in my hands I know already that is valid, because I can’t create an invalid Order object.

## Register for Updates

Now that we know how to create an Order we need to wrap the event logic into a more sophisticated object. We should have a sort of _[Event broker](http://en.wikipedia.org/wiki/Message_broker) also known as Message broker_ able to monitor for events globally. 

Why? Because I can imagine that in my CQRS architecture I will have a process manager that will receive commands and execute them in sequence; while the commands will execute the process manager will also interact with the events raised by the Domain Models involved in the process.

I followed the article of [Udi Dahan](http://www.udidahan.com/) available [here](http://www.udidahan.com/2009/06/14/domain-events-salvation/) and I found a nice and brilliant solution of creating objects that act like _Domain Events_.

The final result is something like this:

<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">void</span> SetState(OrderState orderState)
{
    state = orderState;
    DomainEvents.Raise(
        <span class="kwrd">new</span> OrderStateChangedEvent(<span class="kwrd">this</span>.orderId, <span class="kwrd">this</span>.state));
}</pre>

> The DomainEvents component is a global component that use an IoC container in order to “resolve” the list of subscribers to a specific event.

## Next problem, notify by e-mail

> When the Order Status changes, we should persist the change **somewhere** and we should also **notify** the environment, probably using a **Message Broker**.

We have multiple options here, I can just picture some of them, for example:

  * We can associate an Action<T> to our event, and raise the action that call an E-mail service 
  * We can create a command handler in a different layer that will send an E-mail 
  * We can create a command handler in a different layer that will send a Message to a Queue, this Queue will redirect the Message to an E-mail service 

The first two options are easier and synchronous, while the third one would be a more **enterprise** solution.

The point is that we should decide **who is responsible** to send the e-mail and _if the Domain Model should be aware of this requirement_.

In my opinion, in our specific case, we have an explicit requirement, **whatever there is a subscribed user, we should notify**. Now, if we are smart we can say _it should notify to a service_ and keep the Domain Model loosely coupled from the E-mail concept.

So we need to provide a mechanism to allow a User to register for an Update and verify that the User receives an E-mail.

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; border-bottom-width: 0px; display: inline; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/08/image_thumb4.png" width="420" height="149" />](http://blog.raffaeu.com/wp-content/uploads/2013/08/image4.png)

I just need a way to provide an E-mail service, a _temporary one_, that I will implement later when I will be done with my domain. In this case [Mocking](http://en.wikipedia.org/wiki/Mock_object) is probably the best option here, and because my DomainEvents manager is providing methods using [C# generics](http://msdn.microsoft.com/en-us/library/512aeb7t(v=vs.80).aspx), I can easily fake **any event handler** that I want:

<pre class="csharpcode">var handlerMock = 
      DomainEvents.Container
         .Resolve&lt;Mock&lt;IHandles&lt;OrderStateChangedEvent&gt;&gt;&gt;();
handlerMock
      .Verify(x =&gt; x.Handle
                        (It.IsAny&lt;OrderStateChangedEvent&gt;()), 
                         Times.Once());</pre>

Now, if you think for a second, the _IHandles_ interface could be any contract:

  * _OrderStateChanged_**Email**Handler 
  * _OrderStateChanged_**Persist**Handler 
  * _OrderStateChanged_**Bus**Handler 

Probably you will have an Infrastructure service that will provide a specific handler, a data layer that will provide another handler and so on. The business logic stays in the Domain Model and the event implementation is completely loosely coupled from the Domain. Every time the Domain _raise_ an event, then the _infrastructure_ will catch it and _broadcast_ it to the appropriate handler(s).

## Conclusion

The sample shown previously is a very simple object graph (1 object) that provides behaviors and data. It fits perfectly into BDD because the business logic is behavior driven thanks to the ubiquitous language and it does not involve any external dependency (services or persistence).

We can test the logic of our Domain in complete isolation without having the disadvantage of an anemic Domain that does not carry any “real” business value. We built a language that can be shared between the team and the analysts and we have tests that _speak_.

Probably an additional functionality could be to design the **Commands** in charge of <u>capture the intent of the user</u> and start to test the complete behavior. So that we will have an infrastructure where the _system_ intercepts _commands_ generated by the User and use these command to _interact_ with the _Domain Model_.

Again, even for Domain Model the rule is the same than any other pattern (_architectural and not_). It does not require a Data Layer, it does not require a Service Bus and so on. The Domain Model should contain behaviors and data, then the external dependencies are <u>external dependencies …</u>

Now, if you will do your homeworks properly, you should be able to have some good _fitness_ around your Domain Model like the following one:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; border-bottom-width: 0px; display: inline; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/08/image_thumb5.png" width="420" height="145" />](http://blog.raffaeu.com/wp-content/uploads/2013/08/image5.png)