---
id: 96
title: 'TypeMock tutorial #03. Control behaviors.'
date: 2011-07-01T23:49:36+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=96
permalink: /archive/2011/07/01/typemock-tutorial-03-control-behaviors.aspx
categories:
  - 'C#'
  - Design Pattern
  - TDD
  - TypeMock
---
<p style="border-bottom: #666666 1px solid; border-left: #666666 1px solid; padding-bottom: 3px; margin: 3px; padding-left: 3px; padding-right: 3px; background: #eee; border-top: #666666 1px solid; border-right: #666666 1px solid; padding-top: 3px; border-radius: 5px; -moz-border-radius: 5px">
  <strong>Note for purists: </strong>In this tutorial I am showing you a <strong>simplified example of a Unit of Work and a Repository</strong>, please do not care about the complexity or simplicity of these objects but look at the TypeMock implementations.
</p>

We are now at the third post of this series and I found out that there are a lot of readers interested in learning <a href="http://www.typemock.com" target="_blank">TypeMock</a>, which means that series will have to continue! 

<a href="http://blog.raffaeu.com/archive/2011/06/25/typemock-tutorial-02-object-creation.aspx" target="_blank">Last time</a>, we have created some object’s mocks using TypeMock but they were simple Value Objects with nothing or very few business logic in it. This time I want to show you how you can control the behaviors of a mock so that you do not have to control or fake the entire object if you are testing a single method.

## Testing a IUnitOfWork

I do not know if you have already created a data layer in your career of software developer; if you did not, you can have a look at one of my tutorials or books about layering an application. 

First of all we have a <a href="http://martinfowler.com/eaaCatalog/unitOfWork.html" target="_blank">Unit of Work</a>, which allows us to _“Save”_, _“Update”_ or _“Delete”_ the object we are passing it using a generic signature. The contract for a Unit of Work is represented by the following image:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/1e9c5526-c5c8-4e89-8a12-ed3e9f1fe12fimage_2.png" rel="lightbox"><img style="background-image: none; border-bottom: 0px; border-left: 0px; margin: 0px 5px 0px 0px; padding-left: 0px; padding-right: 0px; display: inline; float: left; border-top: 0px; border-right: 0px; padding-top: 0px" title="image" border="0" alt="image" align="left" src="http://raffaeu.com/wp-content/uploads/2013/03/9f0daf1e-cb0b-4e24-b105-2dbb45f9463aimage_thumb.png" width="187" height="183" /></a>

As you can see we have a simple interface with three methods and we still do not have an implementation for it but we have some **expectations** that we would like to pre-test using a mock in order to be sure that the next step will be properly handled by TypeMock.

The **pre-requisite** is that every **entity** in our domain has some properties inherited by a base class DomainObject; these properties can tell us the **ID** of the entity, if the **entity** is **new, modified** or **deleted**.

The following object represents the base class for a domain entity.

<a href="http://raffaeu.com/wp-content/uploads/2013/03/d87286f6-9d66-4aa5-b7e2-1b061e376543image_4.png" rel="lightbox"><img style="background-image: none; border-bottom: 0px; border-left: 0px; margin: 0px 5px 0px 0px; padding-left: 0px; padding-right: 0px; display: inline; float: left; border-top: 0px; border-right: 0px; padding-top: 0px" title="image" border="0" alt="image" align="left" src="http://raffaeu.com/wp-content/uploads/2013/03/f56de5eb-b024-48d5-80fb-40f39276e70bimage_thumb_1.png" width="187" height="200" /></a>

The 3 properties are of type boolean while the UniqueId is of type Guid, so by default we will have a Guid.Empty value and after we mark dirty or updated the object we should have them populated.

 

 

 

 

 

## Test the interface

If we test the interface we can start by writing three different expectations like the three following snippets:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:c6d23991-ac79-4ca3-995d-8795a3b0c70b" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Mark a new entity
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Test</span><span style="color:#ffffff">]</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Category</span><span style="color:#ffffff">(</span><span style="color:#a5c25c">&#8220;DataLayer&#8221;</span><span style="color:#ffffff">)]</span>
        </li>
        <li>
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> CanMarkANewEntityToNewAndChangeItsId()</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">{</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Person</span><span style="color:#ffffff"> person = </span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.Fake.Instance<</span><span style="color:#ffc66d">Person</span><span style="color:#ffffff">>();</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#6897bb">IUnitOfWork</span><span style="color:#ffffff"> uow = </span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.Fake.Instance<</span><span style="color:#6897bb">IUnitOfWork</span><span style="color:#ffffff">>();</span>
        </li>
        <li>
              <span style="color:#ffffff">uow.MarkNew(person);</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(person, </span><span style="color:#ffc66d">Has</span><span style="color:#ffffff">.Property(</span><span style="color:#a5c25c">&#8220;UniqueId&#8221;</span><span style="color:#ffffff">).Not.EqualTo(</span><span style="color:#6897bb">Guid</span><span style="color:#ffffff">.Empty));</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(person, </span><span style="color:#ffc66d">Has</span><span style="color:#ffffff">.Property(</span><span style="color:#a5c25c">&#8220;IsNew&#8221;</span><span style="color:#ffffff">).True);</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">}</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

And as soon as we run this test it simply fails because of course TypeMock is not able to properly mock the method MarkNew as we did not instruct it on how to do it …

The solution in this case is pretty straightforward, before invoking the MarkNew<T> method we need to teach to TypeMock what is our expectation for this method when we add a Person object to it.</p> 

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:21544d38-b692-48fe-8fcb-5c9b88216386" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      DoInstead()
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#ffffff"></span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.WhenCalled(() => </span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">uow.MarkNew(person))</span>
        </li>
        <li>
              <span style="color:#ffffff">.DoInstead(callContext =></span>
        </li>
        <li style="background: #0c0c0c">
                             <span style="color:#ffffff">{</span>
        </li>
        <li>
                                 <span style="color:#ffffff"></span><span style="color:#cc7832">var</span><span style="color:#ffffff"> p = callContext.Parameters[</span><span style="color:#6897bb"></span><span style="color:#ffffff">] </span><span style="color:#cc7832">as</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">Person</span><span style="color:#ffffff">;</span>
        </li>
        <li style="background: #0c0c0c">
                                 <span style="color:#ffffff">p.UniqueId = </span><span style="color:#6897bb">Guid</span><span style="color:#ffffff">.NewGuid();</span>
        </li>
        <li>
                                 <span style="color:#ffffff">p.IsNew = </span><span style="color:#cc7832">true</span><span style="color:#ffffff">;</span>
        </li>
        <li style="background: #0c0c0c">
                                 <span style="color:#ffffff"></span><span style="color:#cc7832">return</span><span style="color:#ffffff"> p;</span>
        </li>
        <li>
                             <span style="color:#ffffff">});</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff"></span><span style="color:#cc7832">var</span><span style="color:#ffffff"> expectedPerson = uow.MarkNew(person);</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

In this case we have informed TypeMock that when we will call the method MarkNew<T> passing as a generic paramenter the Person object, it will have to modify the person object and return it with a new ID and the IsNew property populated.

Another way to do that is to use the WillReturn method of TypeMock that can be used, like in this case when we have functions and not void methods.

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:4f4ecd34-ef1c-4561-aedb-94b6e0e8e8c0" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      WillReturn
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#ffffff">person.UniqueId = </span><span style="color:#6897bb">Guid</span><span style="color:#ffffff">.NewGuid();</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">person.IsNew = </span><span style="color:#cc7832">true</span><span style="color:#ffffff">;</span>
        </li>
        <li>
          <span style="color:#ffffff"></span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.WhenCalled(() => uow.MarkNew<</span><span style="color:#ffc66d">Person</span><span style="color:#ffffff">>(person)).WillReturn(person);</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff"></span><span style="color:#cc7832">var</span><span style="color:#ffffff"> expectedPerson = uow.MarkNew(person);</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

In the same way we can test that the method may also return an unexpected exception, so we can inform TypeMock to force the mock interface to throw an exception.

This section of type mock is called **_Controlling method behaviors_** and you can find a detailed documentation about it at this address:

<a href="http://docs.typemock.com/isolator/##typemock.chm/Documentation/SettingBehaviorAAA.html" target="_blank">controlling methods</a>

In the next tutorial we will see how to customize a chain of mockup object and faking the methods so that the IUnitOfWork will be used as a dependency for a Repository class.

If you want you can also download the code of every tutorial at this address on Codeplex:

<http://typemock.codeplex.com/>

<img style="border-bottom-style: none; border-left-style: none; border-top-style: none; border-right-style: none" class="wlEmoticon wlEmoticon-winkingsmile" alt="Winking smile" src="http://raffaeu.com/wp-content/uploads/2013/03/9eccd053-31b7-4db9-8840-b8d53e965552wlEmoticon-winkingsmile_2.png" />

**Encrypted string of the week:** IHcKGzESRVs=      **  
using Blowfish CBC 64bit**