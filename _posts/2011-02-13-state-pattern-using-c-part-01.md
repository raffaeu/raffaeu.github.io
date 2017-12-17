---
id: 153
title: 'State pattern using C#. Part 01'
date: 2011-02-13T14:46:18+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=153
permalink: /archive/2011/02/13/state-pattern-using-c-part-01.aspx
categories:
  - 'C#'
  - Design Pattern
  - MVVM
  - WPF
---
I have been busy for a while writing my two books about MVVM and WPF but I am almost done so be ready to get more posts in the next months. This one is the first of a series that I will write to solve the **state pattern** issue.

Today I want to start to talk about the <a href="http://en.wikipedia.org/wiki/State_pattern" target="_blank">state pattern</a>, a design pattern used to represent the state of an object and how we can apply this pattern in a normal WPF application.

<font style="background-color: #dfce04"><strong><em>The problem we have is that based on the state of an Order we can or we can’t execute a specific action.</em></strong></font>

Before starting to talk about the pattern we need a sample application, right? So, what is better than having a nice WPF application that we will use to represents the problem? <img style="border-bottom-style: none; border-right-style: none; border-top-style: none; border-left-style: none" class="wlEmoticon wlEmoticon-smile" alt="Smile" src="http://raffaeu.com/wp-content/uploads/2013/03/d3db15ad-cb17-4956-b825-fcc6da2d6622wlEmoticon-smile_2.png" />

### Process an order using States

The example I want to use is the classic Order entity that during the order process can be moved to different states. The following diagram create with Visual Studio shows you what I am talking about:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/5240244e-10d4-47e5-9a3b-c2d393906c6cStateDiagram_2.png" rel="lightbox"><img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="StateDiagram" border="0" alt="StateDiagram" src="http://raffaeu.com/wp-content/uploads/2013/03/986e4c21-36a0-4eec-9740-63f9f87bb2e8StateDiagram_thumb.png" width="484" height="212" /></a>

The previous image shows the state diagram applied to an Order:

  * You can **create**  an order and after it is created the state is of type **Created** 
  * Then you can **modified** the order or you can **cancel** the order; if you **cancel** the order, its state is **cancelled** and you can’t do anything anymore 
  * An order that has been **created** can be **approved** and its state will change to **approved** 

So in the previous Use Case we have identified 4 actions (blue) and 3 states (white), for each action there is a specific state, in a specific state you can execute only a specific or a set of specific actions and from one state you can move only to one or more specific states. For instance, from the Approved state we can’t rollback to the Created state and so on …

Now, how would you express the previous diagram using a Domain Model composed by an Order entity and a State property? First thing first is to implement a Domain Entity.

<a href="http://raffaeu.com/wp-content/uploads/2013/03/48eee0fe-9458-4d52-9bb5-1e43335a5e30OrderObject_2.png" rel="lightbox"><img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="OrderObject" border="0" alt="OrderObject" src="http://raffaeu.com/wp-content/uploads/2013/03/b2b51c1a-809a-48e0-9411-8b40b94bf49aOrderObject_thumb.png" width="280" height="189" /></a>

Now we can implement the **classic state pattern** described by Martin Fowler.

### Classic implementation of the State Pattern

If you are an MVVM developer you may believe that the first and almost the easiest way of implementing this pattern is to use the <a href="http://en.wikipedia.org/wiki/Command_pattern" target="_blank">Command pattern</a>, right? So, if we plan to extend that in the Order entity we should have a model like the following one:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/1cbe34a6-1b80-4be0-9780-dcda733476a9CommandPattern_2.png" rel="lightbox"><img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="CommandPattern" border="0" alt="CommandPattern" src="http://raffaeu.com/wp-content/uploads/2013/03/ea3be4da-6675-4061-bd5b-aa129366ed74CommandPattern_thumb.png" width="315" height="223" /></a>

Where the command implementation may be something like this:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:593950ba-b5df-4eba-b9c4-460a0ebcad50" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Command Pattern
    </div>
    
    <div style="background: #ddd; max-height: 500px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          CreateOrder = <span style="color:#0000ff">new</span> <span style="color:#2b91af">Command</span>(
        </li>
        <li style="background: #f3f3f3">
              () => <span style="color:#0000ff">true</span>,
        </li>
        <li>
              () =>
        </li>
        <li style="background: #f3f3f3">
                  {
        </li>
        <li>
                      Code = <span style="color:#a31515">&#8220;ABC123&#8221;</span>;
        </li>
        <li style="background: #f3f3f3">
                      State = <span style="color:#2b91af">OrderState</span>.Created;
        </li>
        <li>
                  }
        </li>
        <li style="background: #f3f3f3">
              );
        </li>
        <li>
          ModifyOrder = <span style="color:#0000ff">new</span> <span style="color:#2b91af">Command</span>(
        </li>
        <li style="background: #f3f3f3">
              () => State == <span style="color:#2b91af">OrderState</span>.Created,
        </li>
        <li>
              () =>
        </li>
        <li style="background: #f3f3f3">
                  {
        </li>
        <li>
                      State = <span style="color:#2b91af">OrderState</span>.Modified;
        </li>
        <li style="background: #f3f3f3">
                  });
        </li>
        <li>
          CancelOrder = <span style="color:#0000ff">new</span> <span style="color:#2b91af">Command</span>(
        </li>
        <li style="background: #f3f3f3">
              () => <span style="color:#0000ff">this</span>.State == <span style="color:#2b91af">OrderState</span>.Created || <span style="color:#0000ff">this</span>.State == <span style="color:#2b91af">OrderState</span>.Modified,
        </li>
        <li>
              () => { <span style="color:#0000ff">this</span>.State = <span style="color:#2b91af">OrderState</span>.Cancelled; });
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

When an order is created, by default, we do not allow any state so the default state is **undefined** and these are the tests:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:ec4e17a9-4310-4adb-adeb-d2f2391da9da" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      TDD &#8211; change state
    </div>
    
    <div style="background: #ddd; max-height: 400px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          [<span style="color:#2b91af">Test</span>]
        </li>
        <li style="background: #f3f3f3">
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">void</span> AssertThatANewOrderIsUndefined()
        </li>
        <li>
          {
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">var</span> order = <span style="color:#0000ff">new</span> <span style="color:#2b91af">Order</span>();
        </li>
        <li>
              <span style="color:#2b91af">Assert</span>.That(order.State, <span style="color:#2b91af">Is</span>.EqualTo(<span style="color:#2b91af">OrderState</span>.Undefined));
        </li>
        <li style="background: #f3f3f3">
          }
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
          [<span style="color:#2b91af">Test</span>]
        </li>
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">void</span> AssertThatANewOrderCanBeCreated()
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">var</span> order = <span style="color:#0000ff">new</span> <span style="color:#2b91af">Order</span>();
        </li>
        <li style="background: #f3f3f3">
              order.CreateOrder.Run();
        </li>
        <li>
              <span style="color:#2b91af">Assert</span>.That(order.State, <span style="color:#2b91af">Is</span>.EqualTo(<span style="color:#2b91af">OrderState</span>.Created));
        </li>
        <li style="background: #f3f3f3">
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

The last step needs to verify that if we try to move from one state to a not allowed state, the action that we try to execute will throw an exception:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:f4fee0e8-7561-48e2-801f-5f35ee809aff" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      TDD &#8211; Not allowed method
    </div>
    
    <div style="background: #ddd; max-height: 400px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          [<span style="color:#2b91af">Test</span>]
        </li>
        <li style="background: #f3f3f3">
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">void</span> AssertThatCannotApproveANewOrderNotCreated()
        </li>
        <li>
          {
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">var</span> order = <span style="color:#0000ff">new</span> <span style="color:#2b91af">Order</span>();
        </li>
        <li>
              <span style="color:#2b91af">Assert</span>.That(order.State, <span style="color:#2b91af">Is</span>.EqualTo(<span style="color:#2b91af">OrderState</span>.Undefined));
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#2b91af">Assert</span>.Throws<<span style="color:#2b91af">NotSupportedException</span>>(() => order.ApproveOrder.Run());
        </li>
        <li>
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

### Conclusion using the Classic method

This technique **is not clean and it is very verbose** but it is absolutely testable, but not maintainable. For instance, if the number of state enum will increase we have to touch all over the code that execute the flow logic and probably we need also to refactor all the commands so we can say that this solution is **fine** but not maintainable at all.

Another gap of this solution is that every time we want to execute a command we need to fire the CanExecute method that may process some “long running” business logic behind:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:c4123541-cf41-43d9-a4c6-a83f49629c77" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Command Run() method
    </div>
    
    <div style="background: #ddd; max-height: 400px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">void</span> Run()
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">if</span> (canExecute == <span style="color:#0000ff">null</span> || canExecute.Invoke())
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">this</span>.execute.Invoke();
        </li>
        <li style="background: #f3f3f3">
              }
        </li>
        <li>
              <span style="color:#0000ff">else</span>
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">throw</span> <span style="color:#0000ff">new</span> <span style="color:#2b91af">NotSupportedException</span>(<span style="color:#a31515">&#8220;The action can&#8217;t be executed.&#8221;</span>);
        </li>
        <li style="background: #f3f3f3">
              }
        </li>
        <li>
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

In the next blog post we will see a different approach. At the end of the series (04 posts) I will provide a WPF application on [www.codeplex.com](http://www.codeplex.com) with the source code posted in this series; please be patient for now as I can’t upload the final source code yet.