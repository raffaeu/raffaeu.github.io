---
id: 104
title: 'TypeMock tutorial #02. Object creation.'
date: 2011-06-25T00:01:13+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=104
permalink: /archive/2011/06/25/typemock-tutorial-02-object-creation.aspx
categories:
  - 'C#'
  - Design Pattern
  - TDD
  - TypeMock
---
In this new  part of the <a href="http://www.typemock.com" target="_blank">TypeMock</a> series I am going to show you how to deal with objects and classes in general, how you can create them and what are (honestly aren’t) the limit of TypeMock on dealing with objects.

First of all I have just drawn down a little domain that I have added to the demo application. I am planning to upload this demo the next week on Codeplex.com so that every geek reading this blog can just go there and download the source code.

## The Demo domain

The domain is a very simple one, we have an abstract base class called **Person,** then we have two concrete classes, an **Employee** and a **Customer** that right now do not have any differences (we will see in the next tutorials why we have two different concrete classes) and then we have a value object **Address** that is composed **only if we provide to it a parent Person object in the constructor.** The Person entity exposes a **read-only IEnumerable** collection of Addresses, so in order to add or remove an address we must use the provided methods AddAddress and RemoveAddress.

The following picture shows the corresponding class diagram of this small domain.

<a href="http://raffaeu.com/wp-content/uploads/2013/03/32c85ac2-b89f-4a77-8c5c-9df9c2f1d0caClassDiagram_2.png" rel="lightbox[Part02]"><img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="ClassDiagram" border="0" alt="ClassDiagram" src="http://raffaeu.com/wp-content/uploads/2013/03/50bce218-58f4-4557-999d-cc87776227bbClassDiagram_thumb.png" width="420" height="292" /></a>

These are the most important piece of code that you may be interested in:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:0670ad2f-6c4c-455b-8de2-148b4577bcb1" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Read-only collection Addresses
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#ffffff"></span><span style="color:#cc7832">private</span><span style="color:#ffffff"> </span><span style="color:#6897bb">IList</span><span style="color:#ffffff"><</span><span style="color:#ffc66d">Address</span><span style="color:#ffffff">> addresses = </span><span style="color:#cc7832">new</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">List</span><span style="color:#ffffff"><</span><span style="color:#ffc66d">Address</span><span style="color:#ffffff">>();</span>
        </li>
        <li style="background: #0c0c0c">
           
        </li>
        <li>
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#6897bb">IEnumerable</span><span style="color:#ffffff"><</span><span style="color:#ffc66d">Address</span><span style="color:#ffffff">> Addresses</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">{</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#cc7832">get</span><span style="color:#ffffff"> { </span><span style="color:#cc7832">return</span><span style="color:#ffffff"> </span><span style="color:#cc7832">this</span><span style="color:#ffffff">.addresses; }</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">}</span>
        </li>
        <li>
           
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> AddAddress(</span><span style="color:#ffc66d">Address</span><span style="color:#ffffff"> address)</span>
        </li>
        <li>
          <span style="color:#ffffff">{</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#cc7832">if</span><span style="color:#ffffff"> (</span><span style="color:#cc7832">this</span><span style="color:#ffffff">.addresses.Contains(address))</span>
        </li>
        <li>
              <span style="color:#ffffff">{</span>
        </li>
        <li style="background: #0c0c0c">
                  <span style="color:#ffffff"></span><span style="color:#cc7832">throw</span><span style="color:#ffffff"> </span><span style="color:#cc7832">new</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">InvalidOperationException</span><span style="color:#ffffff">(</span><span style="color:#a5c25c">&#8220;The address is already in the collection.&#8221;</span><span style="color:#ffffff">);</span>
        </li>
        <li>
              <span style="color:#ffffff">}</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#cc7832">this</span><span style="color:#ffffff">.addresses.Add(address);</span>
        </li>
        <li>
          <span style="color:#ffffff">}</span>
        </li>
        <li style="background: #0c0c0c">
           
        </li>
        <li>
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> RemoveAddress(</span><span style="color:#ffc66d">Address</span><span style="color:#ffffff"> address)</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">{</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#cc7832">if</span><span style="color:#ffffff"> (!</span><span style="color:#cc7832">this</span><span style="color:#ffffff">.addresses.Contains(address))</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">{</span>
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#cc7832">throw</span><span style="color:#ffffff"> </span><span style="color:#cc7832">new</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">InvalidOperationException</span><span style="color:#ffffff">(</span><span style="color:#a5c25c">&#8220;The address is not in the collection.&#8221;</span><span style="color:#ffffff">);</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">}</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#cc7832">this</span><span style="color:#ffffff">.addresses.Add(address);</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">}</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

and

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:e93a584f-920b-4a56-8d5f-3c7c7ea1b07a" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Constructor of an Address obj
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> Address(</span><span style="color:#ffc66d">Person</span><span style="color:#ffffff"> person)</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">{</span>
        </li>
        <li>
              <span style="color:#ffffff">Person = person;</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">}</span>
        </li>
        <li>
           
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">Person</span><span style="color:#ffffff"> Person { </span><span style="color:#cc7832">get</span><span style="color:#ffffff">; </span><span style="color:#cc7832">private</span><span style="color:#ffffff"> </span><span style="color:#cc7832">set</span><span style="color:#ffffff">; }</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

As you can see we have few things that need to be tested but in order to do that we have to create new instances of these objects in order to run our tests, which is pretty **verbose** and **boring** …

## Create the test project

The first step is to create a new Visual Studio 2010 Visual C# class library project and call it **TypeMockDemo.Fixture** and add the following references to it:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/56c66638-2e69-48ef-8822-f476c4673195image_2.png" rel="lightbox[part02]"><img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/44aeceac-b4af-43b6-9f51-06d5f2170c01image_thumb.png" width="244" height="260" /></a>

The references are pointing to:

  * my TDD framework nUnit (you can work with any TDD framework but I personally found nUnit to be the best out there …)
  * TypeMock assemblies, installed in the GAC of your machine
  * The TypeMockDemo project (the one we have the domain entities in)

Now we can start to create the first class fixture and verify that we can create a new Person, a new Employee and a new Customer. But **hold on a second!** How can we mock an abstract and two sealed class with a mocking framework? We simply can’t _if we are not using TypeMock … <img style="border-bottom-style: none; border-left-style: none; border-top-style: none; border-right-style: none" class="wlEmoticon wlEmoticon-winkingsmile" alt="Winking smile" src="http://raffaeu.com/wp-content/uploads/2013/03/fb39f43a-4d46-45e4-aad4-8191e69d4557wlEmoticon-winkingsmile_2.png" />_

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:c7c837bc-b77c-4224-b2da-49766df8eb76" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Create new abstract and Sealed
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Test</span><span style="color:#ffffff">]</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Category</span><span style="color:#ffffff">(</span><span style="color:#a5c25c">&#8220;Domain.Isolated&#8221;</span><span style="color:#ffffff">)]</span>
        </li>
        <li>
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> AssertThatCanCreateANewPerson()</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">{</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Person</span><span style="color:#ffffff"> person = </span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.Fake.Instance<</span><span style="color:#ffc66d">Person</span><span style="color:#ffffff">>();</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(person, </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.Not.Null);</span>
        </li>
        <li>
          <span style="color:#ffffff">}</span>
        </li>
        <li style="background: #0c0c0c">
           
        </li>
        <li>
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Test</span><span style="color:#ffffff">]</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Category</span><span style="color:#ffffff">(</span><span style="color:#a5c25c">&#8220;Domain.Isolated&#8221;</span><span style="color:#ffffff">)]</span>
        </li>
        <li>
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> AssertThatCanCreateANewEmployee()</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">{</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Person</span><span style="color:#ffffff"> person = </span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.Fake.Instance<</span><span style="color:#ffc66d">Employee</span><span style="color:#ffffff">>();</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(person, </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.Not.Null);</span>
        </li>
        <li>
          <span style="color:#ffffff">}</span>
        </li>
        <li style="background: #0c0c0c">
           
        </li>
        <li>
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Test</span><span style="color:#ffffff">]</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Category</span><span style="color:#ffffff">(</span><span style="color:#a5c25c">&#8220;Domain.Isolated&#8221;</span><span style="color:#ffffff">)]</span>
        </li>
        <li>
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> AssertThatCanCreateANewCustomer()</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">{</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Person</span><span style="color:#ffffff"> person = </span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.Fake.Instance<</span><span style="color:#ffc66d">Customer</span><span style="color:#ffffff">>();</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(person, </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.Not.Null);</span>
        </li>
        <li>
          <span style="color:#ffffff">}</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

Looking at the code we have introduced the new method **Isolate.Fake.Instance<T>** that is coming from TypeMock. With this method we can simply inform TypeMock that we want it will create for us a **Proxy of the object we want to mock** and it will return a derived class of the tested one, **even if we are mocking a sealed class**.

If the class is sealed TypeMock will create a new instance of the original object while if the object is abstract, TypeMock will create a proxy version of that object. Same thing will be done for all the child properties, complex or not …

<a href="http://raffaeu.com/wp-content/uploads/2013/03/913bdef2-3b28-4efa-a508-29d04966dff4image_4.png" rel="lightbox[part02]"><img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/37a7c4e4-f12f-4923-8bb5-b509cc07851dimage_thumb_1.png" width="420" height="163" /></a>

That’s simply **wow**, we just used two lines of code to create a mockup and test it.

Now let’s move forward and let’s verify that we will not be able to add the same address twice and to remove the same address twice from a Person object.

## Working with Instances or Proxy?

First of all we start to create this simple test but the result is not the one we expect … 

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:20c172ff-a4df-4603-a19d-dc40d2650852" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Test a collection
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Test</span><span style="color:#ffffff">]</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Category</span><span style="color:#ffffff">(</span><span style="color:#a5c25c">&#8220;Domain.Isolated&#8221;</span><span style="color:#ffffff">)]</span>
        </li>
        <li>
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> AssertThatCanAddTheSameAddressTwice()</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">{</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Person</span><span style="color:#ffffff"> person = </span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.Fake.Instance<</span><span style="color:#ffc66d">Person</span><span style="color:#ffffff">>();</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Address</span><span style="color:#ffffff"> address = </span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.Fake.Instance<</span><span style="color:#ffc66d">Address</span><span style="color:#ffffff">>();</span>
        </li>
        <li>
              <span style="color:#ffffff">person.AddAddress(address);</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(person.Addresses.Count(), </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.EqualTo(</span><span style="color:#6897bb">1</span><span style="color:#ffffff">));</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.Throws<</span><span style="color:#ffc66d">InvalidOperationException</span><span style="color:#ffffff">>(() => person.AddAddress(address));</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">}</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

nUnit bombs saying that at line 8 the expected result is supposed to be 1 but in reality is 0. Why? This happens because TypeMock has created a full mockup proxy of the person object so also the methods AddAddress and RemoveAddress are mocks and they do not point to the real code we have implemented …

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:de815cd7-9a0d-4757-8662-3c16e300f808" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Control the creation
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Test</span><span style="color:#ffffff">]</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Category</span><span style="color:#ffffff">(</span><span style="color:#a5c25c">&#8220;Domain.Isolated&#8221;</span><span style="color:#ffffff">)]</span>
        </li>
        <li>
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> AssertThatCanAddTheSameAddressTwice()</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">{</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Person</span><span style="color:#ffffff"> person = </span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.Fake.Instance<</span><span style="color:#ffc66d">Person</span><span style="color:#ffffff">>(</span><span style="color:#6897bb">Members</span><span style="color:#ffffff">.CallOriginal, </span><span style="color:#6897bb">ConstructorWillBe</span><span style="color:#ffffff">.Called);</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Address</span><span style="color:#ffffff"> address = </span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.Fake.Instance<</span><span style="color:#ffc66d">Address</span><span style="color:#ffffff">>();</span>
        </li>
        <li>
              <span style="color:#ffffff">person.AddAddress(address);</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(person.Addresses.Count(), </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.EqualTo(</span><span style="color:#6897bb">1</span><span style="color:#ffffff">));</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.Throws<</span><span style="color:#ffc66d">InvalidOperationException</span><span style="color:#ffffff">>(() => person.AddAddress(address));</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">}</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

If we change the way TypeMock is creating the object Person, we can now say to it:

“**_Dear TypeMock, I want that you create an instance of my object and that you call its constructor so that I can test the code I have implemented in this abstract class …_**”

Et voila’, the test will pass! Same thing for the remove address and so on …

Now, the last test we may require is that we want to be sure that when we create a new address, the constructor is properly injecting the parent Person object so that we can keep a back-forward reference from the parent object and the collection of children.

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:40df3ac7-2e44-4b2e-a0b0-e9dbb29f9d91" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Test injection in constructor
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Test</span><span style="color:#ffffff">]</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Category</span><span style="color:#ffffff">(</span><span style="color:#a5c25c">&#8220;Domain.Isolated&#8221;</span><span style="color:#ffffff">)]</span>
        </li>
        <li>
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> AssertThatWhenCreateAnAddressTheParentPersonIsInjected()</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">{</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Person</span><span style="color:#ffffff"> person = </span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.Fake.Instance<</span><span style="color:#ffc66d">Person</span><span style="color:#ffffff">>(</span><span style="color:#6897bb">Members</span><span style="color:#ffffff">.CallOriginal, </span><span style="color:#6897bb">ConstructorWillBe</span><span style="color:#ffffff">.Called);</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Address</span><span style="color:#ffffff"> address = </span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.Fake.Instance<</span><span style="color:#ffc66d">Address</span><span style="color:#ffffff">>();</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(person, </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.Not.Null);</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(address, </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.Not.Null);</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(address, </span><span style="color:#ffc66d">Has</span><span style="color:#ffffff">.Property(</span><span style="color:#a5c25c">&#8220;Person&#8221;</span><span style="color:#ffffff">).EqualTo(person));</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">}</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

We run the test and kabum! It fails again. This time it fails on the address side because the **Person** instance TypeMock is injecting is not the same it returned to use in the previous line of code. So what can we do now?

We can manually create an Address and inject the parent Person but it sucks … or we can do this:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:e31a1af5-a8c2-4c43-8be2-9ba621b1db9f" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Customize constructor
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Test</span><span style="color:#ffffff">]</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">Category</span><span style="color:#ffffff">(</span><span style="color:#a5c25c">&#8220;Domain.Isolated&#8221;</span><span style="color:#ffffff">)]</span>
        </li>
        <li>
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> AssertThatWhenCreateAnAddressTheParentPersonIsInjected()</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff">{</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Person</span><span style="color:#ffffff"> person = </span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.Fake.Instance<</span><span style="color:#ffc66d">Person</span><span style="color:#ffffff">>(</span><span style="color:#6897bb">Members</span><span style="color:#ffffff">.CallOriginal, </span><span style="color:#6897bb">ConstructorWillBe</span><span style="color:#ffffff">.Called);</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Address</span><span style="color:#ffffff"> address = </span><span style="color:#ffc66d">Isolate</span><span style="color:#ffffff">.Fake.Instance<</span><span style="color:#ffc66d">Address</span><span style="color:#ffffff">>(</span><span style="color:#6897bb">Members</span><span style="color:#ffffff">.CallOriginal, </span><span style="color:#6897bb">ConstructorWillBe</span><span style="color:#ffffff">.Called, person);</span>
        </li>
        <li>
              <span style="color:#ffffff"></span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(person, </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.Not.Null);</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(address, </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.Not.Null);</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(address, </span><span style="color:#ffc66d">Has</span><span style="color:#ffffff">.Property(</span><span style="color:#a5c25c">&#8220;Person&#8221;</span><span style="color:#ffffff">).EqualTo(person));</span>
        </li>
        <li>
          <span style="color:#ffffff">}</span>
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

We simply inject the person value we want to use (the one created by TypeMock) because this is what it is going to happen with live code. What we care here is to be sure that inside the Address class constructor, the Person parameter is passed to the read-only property Person of the Address class, nothing more, nothing less!

## Conclusion

As you can see, TypeMock is pretty cool and it allows you to control the way we can create and fake objects. Even if we use proxies we can still ask to TypeMock to create a real mock that reflect our code so that we can still test the business logic included in our objects without the need of creating complex objects manually.

If you want to read more about this topic I kindly suggest you this:

  * <a href="http://docs.typemock.com/Isolator/#%23typemock.chm/Documentation/CreatingFakesWithAAA.html" target="_blank">Faking instances of an object</a>
  * <a href="http://docs.typemock.com/Isolator/#%23typemock.chm/Documentation/ConstructorArgumentsAAA.html" target="_blank">Handling object constructor</a>

In the next tutorial we will see how to customize the methods and other behaviors of an object and I will also publish the first part of the quiz that will allow you to win almost 1,000 USD value of TypeMock license!

Stay tuned