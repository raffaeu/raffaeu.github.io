---
id: 124
title: Unity and injection with factories.
date: 2011-06-12T22:19:36+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=124
permalink: /archive/2011/06/12/unity-and-injection-with-factories.aspx
categories:
  - 'C#'
  - Design Pattern
---
Last week in the office I just found a bug on how I was implementing a series of Inversion of Control chain using Microsoft Unity. To be precise, the bug has been found by one of the new guy in the team, Gary McLean Hall, the author of the book _<a href="http://www.apress.com/9781430231622" target="_blank">“APRESS &#8211; Pro WPF and Silverlight MVVM”</a>._

Before starting with the explanation of the problem, let’s see what I am talking about. I believe that anyone of you know already concepts like “<a href="http://martinfowler.com/articles/injection.html" target="_blank">Inversion of Control</a>” and ‘<a href="http://msdn.microsoft.com/en-us/magazine/cc163739.aspx" target="_blank">Dependency Injection</a>”; if you don’t, just follow the links. <img style="border-bottom-style: none; border-left-style: none; border-top-style: none; border-right-style: none" class="wlEmoticon wlEmoticon-winkingsmile" alt="Winking smile" src="http://raffaeu.com/wp-content/uploads/2013/03/b27c51c5-d596-43ef-ba14-c6d057a1b999wlEmoticon-winkingsmile_2.png" />

### A classic injection mechanism.

A classic mechanism of IoC is the one used by the Unit of Work and Repository patterns, where a repository can’t exist without an injected unit of work able to control the transaction. So, in order to have such a kind of **example**, I have implemented a simple data layer that shows you these dependencies:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/8767192e-6082-479e-88ee-242f5328b127image_2.png" rel="lightbox[ioc_dynamic]"><img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/dc46c365-0693-4af6-8323-d716ff91bf38image_thumb.png" width="368" height="226" /></a>

Now, for these two **contracts** we will need to **concrete implementations** like the following classes:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/5f6652e7-d9f5-4e6f-8557-e4bd81d26d20image_4.png" rel="lightbox[ioc_dynamic]"><img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/80dacd4e-b668-422a-92a2-fd2f9aa14dbeimage_thumb_1.png" width="368" height="226" /></a>

And the final touch will be the implementation. Now, I am not going to implement a full data layer in this post as it is not the target of this writing, instead I want to show you how to “inject” a unit of work …

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:8787b171-f281-4477-95aa-4f46d6d010a7" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Repository constructor
    </div>
    
    <div style="background: #ddd; max-height: 500px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#cc7832">using</span><span style="color:#ffffff"> System;</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#cc7832">using</span><span style="color:#ffffff"> System.Linq;</span>
        </li>
        <li>
           
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#cc7832">namespace</span><span style="color:#ffffff"> InversionOfControl_dynamic</span>
        </li>
        <li>
          <span style="color:#ffffff">{</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">sealed</span><span style="color:#ffffff"> </span><span style="color:#cc7832">class</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">Repository</span><span style="color:#ffffff"> : </span><span style="color:#6897bb">IRepository</span>
        </li>
        <li>
              <span style="color:#ffffff">{</span>
        </li>
        <li style="background: #0c0c0c">
                  <span style="color:#da4832">#region</span><span style="color:#ffffff"> IRepository Members</span>
        </li>
        <li>
           
        </li>
        <li style="background: #0c0c0c">
                  <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> Create<T>(T entity)</span>
        </li>
        <li>
                  <span style="color:#ffffff">{</span>
        </li>
        <li style="background: #0c0c0c">
                      <span style="color:#ffffff"></span><span style="color:#cc7832">this</span><span style="color:#ffffff">.unitOfWork.MarkNew(entity);</span>
        </li>
        <li>
                  <span style="color:#ffffff">}</span>
        </li>
        <li style="background: #0c0c0c">
           
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#808080">///</span><span style="color:#008000"> IRepository implementation removed to save space &#8230;</span>
        </li>
        <li style="background: #0c0c0c">
           
        </li>
        <li>
                  <span style="color:#da4832">#endregion</span>
        </li>
        <li style="background: #0c0c0c">
           
        </li>
        <li>
                  <span style="color:#da4832">#region</span><span style="color:#ffffff"> Injection of the Unit of Work</span>
        </li>
        <li style="background: #0c0c0c">
           
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#cc7832">private</span><span style="color:#ffffff"> </span><span style="color:#6897bb">IUnitOfWork</span><span style="color:#ffffff"> unitOfWork;</span>
        </li>
        <li style="background: #0c0c0c">
           
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> Repository(</span><span style="color:#6897bb">IUnitOfWork</span><span style="color:#ffffff"> unitOfWork)</span>
        </li>
        <li style="background: #0c0c0c">
                  <span style="color:#ffffff">{</span>
        </li>
        <li>
                      <span style="color:#ffffff"></span><span style="color:#cc7832">this</span><span style="color:#ffffff">.unitOfWork = unitOfWork;</span>
        </li>
        <li style="background: #0c0c0c">
                  <span style="color:#ffffff">}</span>
        </li>
        <li>
           
        </li>
        <li style="background: #0c0c0c">
                  <span style="color:#da4832">#endregion</span>
        </li>
        <li>
           
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">}</span>
        </li>
        <li>
          <span style="color:#ffffff">}</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

Now we can easily register the components with Unity and verify that when we call a **Repository** class, we are injecting a new **Unit of Work** using Unity.

The following test is accomplishing this task:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:f2e4438c-c87e-4ecb-a97a-015b174571c2" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      TDD the injection process
    </div>
    
    <div style="background: #ddd; max-height: 500px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">TestFixture</span><span style="color:#ffffff">]</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">sealed</span><span style="color:#ffffff"> </span><span style="color:#cc7832">class</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">StandardFixture</span>
        </li>
        <li>
          <span style="color:#ffffff">{</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#cc7832">private</span><span style="color:#ffffff"> </span><span style="color:#6897bb">IUnityContainer</span><span style="color:#ffffff"> container;</span>
        </li>
        <li>
           
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">[</span><span style="color:#ffc66d">TestFixtureSetUp</span><span style="color:#ffffff">]</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> InitializeTests()</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">{</span>
        </li>
        <li>
                  <span style="color:#ffffff">container = </span><span style="color:#cc7832">new</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">UnityContainer</span><span style="color:#ffffff">();</span>
        </li>
        <li style="background: #0c0c0c">
                  <span style="color:#ffffff">container.RegisterType<</span><span style="color:#6897bb">IUnitOfWork</span><span style="color:#ffffff">, </span><span style="color:#ffc66d">UnitOfWork</span><span style="color:#ffffff">>();</span>
        </li>
        <li>
                  <span style="color:#ffffff">container.RegisterType<</span><span style="color:#6897bb">IRepository</span><span style="color:#ffffff">, </span><span style="color:#ffc66d">Repository</span><span style="color:#ffffff">>();</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">}</span>
        </li>
        <li>
           
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">[</span><span style="color:#ffc66d">Test</span><span style="color:#ffffff">]</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> Assert_That_Unity_Can_Resolve_Dependency()</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">{</span>
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#6897bb">IRepository</span><span style="color:#ffffff"> repository = container.Resolve<</span><span style="color:#6897bb">IRepository</span><span style="color:#ffffff">>();</span>
        </li>
        <li style="background: #0c0c0c">
                  <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(repository, </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.Not.Null);</span>
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(repository, </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.InstanceOf<</span><span style="color:#ffc66d">Repository</span><span style="color:#ffffff">>());</span>
        </li>
        <li style="background: #0c0c0c">
                  <span style="color:#ffffff"></span><span style="color:#808080">// Here I have created a public read-only property</span>
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#808080">// for testing purposes</span>
        </li>
        <li style="background: #0c0c0c">
                  <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(repository, </span><span style="color:#ffc66d">Has</span><span style="color:#ffffff">.Property(</span><span style="color:#a5c25c">&#8220;UnitOfWork&#8221;</span><span style="color:#ffffff">).Not.Null);</span>
        </li>
        <li>
              <span style="color:#ffffff">}</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">}</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

So far so good. The only missing part here is the last statement (line 22) of the code. Here I have created a read-only IUnitOfWork property, **only in the concrete Repository class**, in order to test that the IUnitOfWork is injected properly.

### Injecting using factory

Now, let’s assume for a second that the previous approach is not valid; it is not valid because we **are not in charge** of creating an _IUnitOfWork_ by our self. We need a **factory** that will create the contract for use.

So, first of all, I am going to create a simple factory that will return, with a static method, an instance of an IUnitOfWork object.

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:3df24795-a95a-49c5-a978-7b67d8987629" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Unit of Work Factory
    </div>
    
    <div style="background: #ddd; max-height: 500px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">sealed</span><span style="color:#ffffff"> </span><span style="color:#cc7832">class</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">Factory</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">{</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">static</span><span style="color:#ffffff"> </span><span style="color:#6897bb">IUnitOfWork</span><span style="color:#ffffff"> GetUnitOfWork()</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">{</span>
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#cc7832">return</span><span style="color:#ffffff"> </span><span style="color:#cc7832">new</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">UnitOfWork</span><span style="color:#ffffff">();</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">}</span>
        </li>
        <li>
          <span style="color:#ffffff">}</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

Now we are screwed because the previous approach does not work anymore…  <img style="border-bottom-style: none; border-left-style: none; border-top-style: none; border-right-style: none" class="wlEmoticon wlEmoticon-greenwithenvy" alt="Green with envy" src="http://raffaeu.com/wp-content/uploads/2013/03/8f118e0d-6b32-4f14-aaa9-8062ec22ebc5wlEmoticon-greenwithenvy_2.png" />If I want to create a new instance of a repository I need to inform Unity, somehow, that it needs to call this method of the factory to create a new instance of an _IUnitOfWork_. But how?

We found the class **InjectionFactory** ables to specify a delegate for the creation of a new object, like this code:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:2c3eebbc-16e5-4f34-a03c-1c980777ce08" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      InjectionFactory declaration
    </div>
    
    <div style="background: #ddd; max-height: 500px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#ffffff">container = </span><span style="color:#cc7832">new</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">UnityContainer</span><span style="color:#ffffff">();</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">container.RegisterType<</span><span style="color:#6897bb">IUnitOfWork</span><span style="color:#ffffff">>(</span><span style="color:#cc7832">new</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">InjectionFactory</span><span style="color:#ffffff">(c => </span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Factory</span><span style="color:#ffffff">.GetUnitOfWork()));</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">container.RegisterType<</span><span style="color:#6897bb">IRepository</span><span style="color:#ffffff">, </span><span style="color:#ffc66d">Repository</span><span style="color:#ffffff">>();</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

Now we can simply **assert** that when we create two instance of an IRepository interface using Unity, it will return two different instances of the IUnitOfWork object:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:c1d66592-dc9b-4ca9-a107-4da2ec270c1f" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Code Snippet
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Test</span><span style="color:#ffffff">]</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> Assert_That_Unity_Can_Resolve_Dependency()</span>
        </li>
        <li>
          <span style="color:#ffffff">{</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#cc7832">var</span><span style="color:#ffffff"> currentRepository = container.Resolve<</span><span style="color:#6897bb">IRepository</span><span style="color:#ffffff">>();</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#cc7832">var</span><span style="color:#ffffff"> currentUoW = currentRepository.GetType().GetProperty(</span><span style="color:#a5c25c">&#8220;UnitOfWork&#8221;</span><span style="color:#ffffff">).GetValue(currentRepository, </span><span style="color:#cc7832">null</span><span style="color:#ffffff">);</span>
        </li>
        <li style="background: #0c0c0c">
           
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#cc7832">var</span><span style="color:#ffffff"> expectedRepository = container.Resolve<</span><span style="color:#6897bb">IRepository</span><span style="color:#ffffff">>();</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#cc7832">var</span><span style="color:#ffffff"> expectedUoW = currentRepository.GetType().GetProperty(</span><span style="color:#a5c25c">&#8220;UnitOfWork&#8221;</span><span style="color:#ffffff">).GetValue(expectedRepository, </span><span style="color:#cc7832">null</span><span style="color:#ffffff">);</span>
        </li>
        <li>
           
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(currentUoW, </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.Not.Null);</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(expectedUoW, </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.Not.Null);</span>
        </li>
        <li style="background: #0c0c0c">
           
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(currentUoW, </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.Not.EqualTo(expectedUoW));</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">}</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

If you want to read more about this object and other way of customizing the way Unity allows you to register objects, you can have a look at this section of MSDN:

  * Microsoft Unity 2.0 <a href="http://msdn.microsoft.com/en-us/library/ff663144.aspx" target="_blank">documentation</a>
  * <a href="http://msdn.microsoft.com/en-us/library/ff662245(v=PandP.20).aspx" target="_blank">InjectionFactory</a> class

Happy IoC to everybody!

<img style="border-bottom-style: none; border-left-style: none; border-top-style: none; border-right-style: none" class="wlEmoticon wlEmoticon-openmouthedsmile" alt="Open-mouthed smile" src="http://raffaeu.com/wp-content/uploads/2013/03/df2907e9-3499-4b25-ae16-d841442b5015wlEmoticon-openmouthedsmile_2.png" />