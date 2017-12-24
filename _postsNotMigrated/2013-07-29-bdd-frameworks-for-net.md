---
id: 519
title: BDD frameworks for NET
date: 2013-07-29T16:03:00+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=519
permalink: /archive/:year/:month/:day/:title/
categories:
  - Agile
  - 'C#'
  - TDD
---
When I work in a project that implies Business Logic and Behaviors I usually prefer to write my unit tests using a [Behavioral approach](http://en.wikipedia.org/wiki/Behavior-driven_development), because in this way I can easily projects my acceptance criteria into readable piece of code.

Right now I am already on [Visual Studio 2013](http://www.microsoft.com/visualstudio/eng/2013-preview) and I am struggling a bit with the internal test runner UX, because it doesn’t really fit the behavioral frameworks I am used to work with.

So I decided to try out some behavioral frameworks with Visual Studio 2013. I have a simple piece of functionality that I want to test. When you create a new Order object, if the Order Id is not provided with the right format, the class Order will throw an **ArgumentException**.

If I want to translate this into a more readable BDD sentence, I would say:

> Given that an Order should be created   
> When I create a new Order   
> Then the Order should be created only if the Order Id has a valid format   
> Otherwise I should get an error\

So let’s see now the different ways of writing this test …

## XBehave

[XBehave](https://github.com/xbehave/xbehave.net/blob/master/readme.md) is probably the first framework I tried for behavioral driven tests. The implementation is easy but in order to get it working properly you have to write your tests on top of [xUnit](http://xunit.codeplex.com/) test framework, and of course you have also to install the [xUnit test runner plugin](http://xunit.codeplex.com/discussions/260634) for Visual Studio.

In order to write a test, you need to provide the Given, When and Then implementations. Each step has a description and a delegate that contains the code to be tested. Finally, each method must be decorated with a [Scenario] attribute.

<pre class="csharpcode">[Scenario]
[Example(<span class="str">"ABC-12345-ABC"</span>)]
<span class="kwrd">public</span> <span class="kwrd">void</span> CreatingAnOrderWithValidData(<span class="kwrd">string</span> orderId, Order order)
{
    _
        .When(<span class="str">"creating a new Order"</span>,
                   () =&gt; order = Order.Create(orderId))            
        .Then(<span class="str">"the Order should be created"</span>,
                   () =&gt; order.Should().NotBeNull());
}</pre>

In this specific case I am using the [Example] attribute to provide dynamic data to my test, so that I can test multiple scenarios in one shot.</p> 

[<img title="image" style="border-top: 0px; border-right: 0px; border-bottom: 0px; border-left: 0px; display: inline" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/07/image_thumb6.png" width="520" height="147" />](http://blog.raffaeu.com/wp-content/uploads/2013/07/image6.png) 

The problem? For each step of my test, Visual Studio test runner identifies a unique test, which is not really true and quite unreadable because this is one method, one test but in Visual Studio I see a test row for each delegate passed to XBehave.

## NSpec

[NSpec](http://nspec.org/) is another behavioral framework that allows you to write human readable tests with C#. The installation is easy, you just need to download the Nuget package, the only problem is the interaction with Visual Studio. In order to run the tests you need to call the test runner of NSpec, which **is not integrated into the test runner of Visual Studio**, so you need to read the test results on a console window, without the availability to interact with the failed tests.

In order to test your code you need to provide the Given,When and Then like with any other behavioral framework. The difference is that this framework is making an extensive use of [magic strings](http://en.wikipedia.org/wiki/Magic_string) and it assumes the developer will do the same.

<pre class="csharpcode"><span class="kwrd">void</span> given_an_order_is_created_with_valid_data()
{
    before = () =&gt; order = Order.Create(<span class="str">"xxx-11111-xxx"</span>);
    it[<span class="str">"the order should be created"</span>] = 
         () =&gt; order.should_not_be_null();
}</pre>

And this is the result you will get in Visual Studio Nuget package console (_as you can see, not really intuitive and easy to manage_):

[<img title="image" style="border-top: 0px; border-right: 0px; border-bottom: 0px; border-left: 0px; display: inline" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/07/image_thumb7.png" width="520" height="150" />](http://blog.raffaeu.com/wp-content/uploads/2013/07/image7.png)&#160; 

Honestly the test syntax is quite nice and readable but I found annoying and silly that I have to run my tests over a console screen, come’n the interaction with Visual Studio UI is quite well structured and this framework should at least prints the output of the tests into the test runner window.</p> 

## StoryQ

I came to [StoryQ](http://storyq.codeplex.com/) just few days ago, I was googling about “_fluent behavioral framework”._ The project is still on [www.codeplex.com](http://www.codeplex.com) but it looks pretty inactive, so take it as is. There are few bugs and code constraints that force you to implement a very thigh code syntax, thigh to the StoryQ conventions.

The syntax is absolutely fluent and it’s very readable, compared to other behavioral frameworks, I guess this is the nearest to the developer style.

<pre class="csharpcode"><span class="kwrd">new</span> StoryQ.Story(<span class="str">"Create Order"</span>)
.InOrderTo(<span class="str">"Create a valid Order"</span>)
.AsA(<span class="str">"User"</span>)
.IWant(<span class="str">"A valid Order object"</span>)

.WithScenario(<span class="str">"Valid order id"</span>)
    .Given(IHaveAValidOrderId)
        .And(OrderIsInitialized)
    .When(ANewOrderIsCreated)
    .Then(TheOrderShouldNotBeNull)

.ExecuteWithReport(MethodBase.GetCurrentMethod());</pre>

And the output is fully integrated into Visual Studio test runner, just include the previous code into a test method of your preferred unit test framework.

[<img title="image" style="border-top: 0px; border-right: 0px; border-bottom: 0px; border-left: 0px; display: inline" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/07/image_thumb8.png" width="320" height="190" />](http://blog.raffaeu.com/wp-content/uploads/2013/07/image8.png) 

The output is exactly what we need, a clear BDD output style with a reference to each step status (succeed, failed, exception …)

## BDDify

[BBDify](https://github.com/TestStack/TestStack.BDDfy/blob/master/readme.md) is the lightest framework of the one I tried out and honestly is also the most readable and easy to implement. You can decide to use the Fluent API or the standard conventions. With both implementations you can easily build a dictionary of steps that can be recycled over and over.

In my case I have created a simple Story in BDDfy and represented the story using C# and no magic strings:

<pre class="csharpcode">[Fact]
<span class="kwrd">public</span> <span class="kwrd">void</span> OrderIsCreatedWithValidId()
{
    <span class="kwrd">this</span>.
        Given(s =&gt; s.OrderIdIsAvailable())
        .When(s =&gt; s.CreateANewOrder())
        .Then(s =&gt; s.OrderShouldNotBeNull())
        .BDDfy(<span class="str">"Create a valid order"</span>);
}</pre>

And the test output really fit into Visual Studio test runner, plus I can also retrieve in the test output my User Story description, so that my QAs can easily interact with the tests results:

[<img title="image" style="border-top: 0px; border-right: 0px; border-bottom: 0px; border-left: 0px; display: inline" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/07/image_thumb9.png" width="370" height="376" />](http://blog.raffaeu.com/wp-content/uploads/2013/07/image9.png) 

I really like this layout style because I can already picture this layout into my TFS reports.

I hope you enjoyed!