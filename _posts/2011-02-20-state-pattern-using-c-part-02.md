---
id: 145
title: 'State pattern using C#. Part 02.'
date: 2011-02-20T14:00:23+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=145
permalink: /archive/2011/02/20/state-pattern-using-c-part-02.aspx
categories:
  - 'C#'
  - Design Pattern
  - WPF
---
In the <a href="http://blog.raffaeu.com/archive/2011/02/13/state-pattern-using-c-part-01.aspx" target="_blank">previous post</a> we saw how we can implement the state pattern (I know, I didn’t show you the purist way of using the State pattern …) and include in the state execution the flow logic.

This technique is fine but … it requires a lot of effort in the implementation and requires a lot of maintenance, plus it has the GAP of forcing us to re-run the CanExecute delegate every time we want to execute a specific action.

On the web I have found some solutions that personally didn’t satisfy me at all. I personally believe that the best way of designing a state machine workflow is to use a workflow engine and NET Framework provides with NET 4 an amazing state engine. Anyway let’s see what the web offers instead of using WF 4.

### Stateless Open source project

Stateless is an Open source project hosted on Google code and available here: [http://code.google.com/p/stateless/](http://code.google.com/p/stateless/ "http://code.google.com/p/stateless/"); it is a C# implementation of a stateless workflow using the BOO language.

We have the same domain exposed in the previous post but in this case we modified a little bit the Order object and we do not use anymore the Command pattern.

<a href="http://raffaeu.com/wp-content/uploads/2013/03/114d632b-0dca-451b-a2ba-d8dcee510a6bOrderObjectStateless_2.png" rel="lightbox"><img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="OrderObjectStateless" border="0" alt="OrderObjectStateless" src="http://raffaeu.com/wp-content/uploads/2013/03/3b4e0e1f-e9bb-41aa-a7a3-e6e60244ddb2OrderObjectStateless_thumb.png" width="303" height="204" /></a>

The class Order has 5 different methods that can modify its state in the following way:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:aeb6023e-17ba-4ba2-82c4-d60a5b7aab64" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Create an Order
    </div>
    
    <div style="background: #ddd; max-height: 400px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">void</span> Create()
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">this</span>.State = <span style="color:#2b91af">OrderState</span>.Created;
        </li>
        <li style="background: #f3f3f3">
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

They do not verify anymore if the action can or cannot be execute, we just know that the Create method, for example, modifies the state of the Order to “Created”. 

Now it is time to wrap this code in a separated class that we will call OrderService and that is identified in DDD as a Domain Service object, a service used in the domain space to wrap business logic and keep it outside the Entity object. The final result should like this one:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/8855e1c9-bbff-4794-9d77-d3a2ad06f6f9statelessdiagram_2.png" rel="lightbox"><img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="statelessdiagram" border="0" alt="statelessdiagram" src="http://raffaeu.com/wp-content/uploads/2013/03/849aceec-cfcd-47c7-a105-506975212128statelessdiagram_thumb.png" width="378" height="233" /></a>

The trick with <a title="http://code.google.com/p/stateless/" href="http://code.google.com/p/stateless/" target="_blank">http://code.google.com/p/stateless/</a> is to split the service in two parts, the first one is used to Bootstrap the stateless framework in the following way:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:5efb5c45-34ee-4114-a0bd-8263be2594c8" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Stateless boostrapping
    </div>
    
    <div style="background: #ddd; max-height: 500px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">sealed</span> <span style="color:#0000ff">class</span> <span style="color:#2b91af">OrderService</span>
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">private</span> <span style="color:#2b91af">StateMachine</span><<span style="color:#2b91af">OrderState</span>, <span style="color:#2b91af">OrderActions</span>> workflow;
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
              <span style="color:#0000ff">public</span> OrderService()
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  workflow = <span style="color:#0000ff">new</span> <span style="color:#2b91af">StateMachine</span><<span style="color:#2b91af">OrderState</span>, <span style="color:#2b91af">OrderActions</span>>(<span style="color:#2b91af">OrderState</span>.Undefined);
        </li>
        <li style="background: #f3f3f3">
                  workflow
        </li>
        <li>
                      .Configure(<span style="color:#2b91af">OrderState</span>.Undefined)
        </li>
        <li style="background: #f3f3f3">
                      .Permit(<span style="color:#2b91af">OrderActions</span>.Create, <span style="color:#2b91af">OrderState</span>.Created);
        </li>
        <li>
                  workflow
        </li>
        <li style="background: #f3f3f3">
                      .Configure(<span style="color:#2b91af">OrderState</span>.Created)
        </li>
        <li>
                      .Permit(<span style="color:#2b91af">OrderActions</span>.Cancel, <span style="color:#2b91af">OrderState</span>.Cancelled)
        </li>
        <li style="background: #f3f3f3">
                      .Permit(<span style="color:#2b91af">OrderActions</span>.Modify, <span style="color:#2b91af">OrderState</span>.Modified)
        </li>
        <li>
                      .Permit(<span style="color:#2b91af">OrderActions</span>.Approve, <span style="color:#2b91af">OrderState</span>.Approved);
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
              }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

And then we add a method in the service that will be used to Fire a specific state change, like this one:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:2539e13a-b94e-4415-a314-eb8f0c99c13f" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Command pattern
    </div>
    
    <div style="background: #ddd; max-height: 400px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">void</span> Fire(<span style="color:#2b91af">Order</span> order, <span style="color:#2b91af">OrderActions</span> action)
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              workflow.Fire(action);
        </li>
        <li style="background: #f3f3f3">
              order.State = workflow.State;
        </li>
        <li>
          }
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">bool</span> CanFire(<span style="color:#2b91af">Order</span> order, <span style="color:#2b91af">OrderActions</span> action)
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">return</span> workflow.CanFire(action);
        </li>
        <li style="background: #f3f3f3">
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

Now, by default, Stateless raises an error (Exception) if the operation can’t be executed. The following test demonstrates the exception raised by stateless:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:fe8ebadf-8562-49e1-aeeb-36ed8d2c87ee" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      TDD
    </div>
    
    <div style="background: #ddd; max-height: 400px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          [<span style="color:#2b91af">Test</span>]
        </li>
        <li style="background: #f3f3f3">
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">void</span> CannotApproveAnOrderBeforeCreatingIt()
        </li>
        <li>
          {
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">var</span> order = <span style="color:#0000ff">new</span> <span style="color:#2b91af">Order</span>();
        </li>
        <li>
              <span style="color:#0000ff">var</span> service = <span style="color:#0000ff">new</span> <span style="color:#2b91af">OrderService</span>();
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#2b91af">Assert</span>.Throws<<span style="color:#2b91af">InvalidOperationException</span>>(() =>
        </li>
        <li>
                  service.Fire(order, <span style="color:#2b91af">OrderActions</span>.Approve));
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#2b91af">Assert</span>.That(order.State, <span style="color:#2b91af">Is</span>.EqualTo(<span style="color:#2b91af">OrderState</span>.Undefined));
        </li>
        <li>
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

The exception is of type InvalidOperationException. 

### Conclusion

Stateless is a good state machine framework, open source, easy to learn and it has a good and clear DSL language. Unfortunately the project is very young, the active developer is only one and it still has a huge list of **_ToDo and Bugs_** to be fixed.

It can be used to replace custom If and Switch in the Domain language but if you need to do some custom and more complicated evaluations, Stateless can result very verbose because it doesn’t have a UI so you have to prepare all the If and Switch using the Configure syntax.