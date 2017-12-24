---
id: 207
title: 'How to write fluent interface with C# and Lambda.'
date: 2010-06-26T19:27:39+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=207
permalink: /archive/2010/06/26/how-to-write-fluent-interface-with-c-and-lambda.aspx
categories:
  - 'C#'
  - Design Pattern
---
Last week I had a nice discussion in the office about “how to write fluent interface” and I have found a couple of articles over the internet about that. As usual I disagree with some of them and, as usual, my friend <a href="http://milestone.topics.it" target="_blank">Mauro Servienti</a> (MVP C#) has a <a href="http://milestone.topics.it/blog/post/fluently.design.me-inganniamo-lintellisense" target="_blank">nice article about it</a>, unfortunately in Italian. He just gave me the **startup input**.

If you think about the Fluent Interface, it is just a trick that you use with C# in order to cheat the intellisense of Visual Studio and in order to create a nice style for your code. Of course there is a specific way to write fluent interface. 

Let’s make a short sample. The classic **Person class** and the factory pattern.

We have a class which represents the Person Entity and has some common properties.

<a href="http://raffaeu.com/wp-content/uploads/2013/03/f9864100-7b2d-40e6-9d7e-9c659b8479a6image_2.png" rel="lightbox[FluentInterface]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/b642ceef-b6d4-4e78-9130-31ff894ff7dcimage_thumb.png" width="447" height="218" /></a> 

Very easy. Now, in a normal world you would have something like this, in order to create a new instance of a class person.

First way, the classis way:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:e9f5c654-3b34-4754-8303-2b36b9fc684f" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Classic Person Factory
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">class</span> <span style="color:#2b91af">PersonFactory</span>
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#2b91af">Person</span> CreatePerson(<span style="color:#0000ff">string</span> firstName, <span style="color:#0000ff">string</span> middleName, <span style="color:#0000ff">string</span> lastName)
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">var</span> person = <span style="color:#0000ff">new</span> <span style="color:#2b91af">Person</span>
        </li>
        <li style="background: #f3f3f3">
                  {
        </li>
        <li>
                      FirstName = firstName,
        </li>
        <li style="background: #f3f3f3">
                      MiddleName = middleName,
        </li>
        <li>
                      LastName = lastName
        </li>
        <li style="background: #f3f3f3">
                  };
        </li>
        <li>
                  <span style="color:#0000ff">return</span> person;
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

This code is classic and very verbose. If you want to use it the syntax would be something like this:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:f12213a8-5ace-47fb-b154-34907cc0adf9" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Create Person
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">var</span> person = <span style="color:#2b91af">PersonFactory</span>.CreatePerson(<span style="color:#a31515">&#8220;Raffaele&#8221;</span>, <span style="color:#0000ff">string</span>.Empty, <span style="color:#a31515">&#8220;Garofalo&#8221;</span>);
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

Now, if we want to add an address to this person we need an additional like of code like this one, and of course a new factory for the class person or a new method in the person factory … whatever …

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:2e58b574-136d-4995-b758-799ccea52171" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Create Address
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">var</span> address = <span style="color:#2b91af">PersonFactory</span>.CreateAddress(<span style="color:#a31515">&#8220;1st Lane&#8221;</span>, <span style="color:#a31515">&#8220;Main Road&#8221;</span>, <span style="color:#a31515">&#8220;Hamilton&#8221;</span>, <span style="color:#a31515">&#8220;Bermuda&#8221;</span>, <span style="color:#a31515">&#8220;HM10&#8221;</span>);
        </li>
        <li style="background: #f3f3f3">
          person.Addresses.Add(address);
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

First of all, here we have a big problem. All the parameters are strings. So, if we don’t explain in a proper verbose way each parameter, the developer that will use our factory won’t be able to know what to write in each parameter. Second thing, if we don’t use C# 4 we have to specify each parameter value anyway … Finally we are avoiding a nice readability in our code.

### The first step for a fluent interface.

I saw a lot of code around the web but a lot of people forget about the name of this technique … The name is Fluent **Interface** so this means that probably we should add some interfaces in our code in order to have a good result. Well VS 2010 is able to create an interface from a class just with a couple of clicks … And this is the final result:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/4336697b-7824-4f64-9495-a7412f8ca535image_4.png" rel="lightbox[FluentInterface]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/60c99180-5147-4bc8-a655-4cc7f6a69315image_thumb_1.png" width="260" height="170" /></a> 

Now we need to translate each method in a Fluent method. Let’s start with the class person. What I am doing is an interface for my Factory and two methods, one for the Factory initialization, where we initialize a new instance of the class person and one to finalize it, where we will return the current instance of that class. Of course we need also the methods to add some values to the person properties. Look at the UML:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/2f4a9244-d636-4eda-b06a-8479063e0e16image_6.png" rel="lightbox[FluentInterface]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/7756ab33-0f8e-498a-b7f0-1cfe13cedc22image_thumb_2.png" width="260" height="199" /></a> 

At here is the code:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:c20722af-9536-4e7b-a9ab-da6953349ccf" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      IPersonFactory
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">interface</span> <span style="color:#2b91af">IPersonFactory</span>
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#2b91af">IPersonFactory</span> Initialize();
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#2b91af">IPersonFactory</span> AddFirstName(<span style="color:#0000ff">string</span> firstName);
        </li>
        <li>
              <span style="color:#2b91af">IPersonFactory</span> AddLastName(<span style="color:#0000ff">string</span> lastName);
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#2b91af">IPersonFactory</span> AddMiddleName(<span style="color:#0000ff">string</span> middleName);
        </li>
        <li>
              <span style="color:#2b91af">IPerson</span> Create();
        </li>
        <li style="background: #f3f3f3">
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

And this is the implementation:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:a42fc165-0035-4410-ae7d-b44623e7e057" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      PersonFactory
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">class</span> <span style="color:#2b91af">PersonFactory</span> : <span style="color:#2b91af">IPersonFactory</span>
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">private</span> <span style="color:#2b91af">IPerson</span> person = <span style="color:#0000ff">null</span>;
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">public</span> <span style="color:#2b91af">IPersonFactory</span> Initialize()
        </li>
        <li>
              {
        </li>
        <li style="background: #f3f3f3">
                  person = <span style="color:#0000ff">new</span> <span style="color:#2b91af">Person</span>();
        </li>
        <li>
                  <span style="color:#0000ff">return</span> <span style="color:#0000ff">this</span>;
        </li>
        <li style="background: #f3f3f3">
              }
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">public</span> <span style="color:#2b91af">IPersonFactory</span> AddFirstName(<span style="color:#0000ff">string</span> firstName)
        </li>
        <li>
              {
        </li>
        <li style="background: #f3f3f3">
                  person.FirstName = firstName;
        </li>
        <li>
                  <span style="color:#0000ff">return</span> <span style="color:#0000ff">this</span>;
        </li>
        <li style="background: #f3f3f3">
              }
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">public</span> <span style="color:#2b91af">IPersonFactory</span> AddLastName(<span style="color:#0000ff">string</span> lastName)
        </li>
        <li>
              {
        </li>
        <li style="background: #f3f3f3">
                  person.LastName = lastName;
        </li>
        <li>
                  <span style="color:#0000ff">return</span> <span style="color:#0000ff">this</span>;
        </li>
        <li style="background: #f3f3f3">
              }
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">public</span> <span style="color:#2b91af">IPersonFactory</span> AddMiddleName(<span style="color:#0000ff">string</span> middleName)
        </li>
        <li>
              {
        </li>
        <li style="background: #f3f3f3">
                  person.MiddleName = middleName;
        </li>
        <li>
                  <span style="color:#0000ff">return</span> <span style="color:#0000ff">this</span>;
        </li>
        <li style="background: #f3f3f3">
              }
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">public</span> <span style="color:#2b91af">IPerson</span> Create()
        </li>
        <li>
              {
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#0000ff">return</span> person;
        </li>
        <li>
              }
        </li>
        <li style="background: #f3f3f3">
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

So now we start to have a FluentInterface capability in our code.

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:d022b871-26df-4050-aa0a-eb1bd0ca0d0f" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Code Snippet
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">var</span> person = <span style="color:#0000ff">new</span> <span style="color:#2b91af">PersonFactory</span>()
        </li>
        <li style="background: #f3f3f3">
                          .Initialize()
        </li>
        <li>
                          .AddFirstName(<span style="color:#a31515">&#8220;Raffaele&#8221;</span>)
        </li>
        <li style="background: #f3f3f3">
                          <span style="color:#008000">// we can skip this line now &#8230;</span>
        </li>
        <li>
                          .AddMiddleName(<span style="color:#0000ff">string</span>.Empty)
        </li>
        <li style="background: #f3f3f3">
                          .AddLastName(<span style="color:#a31515">&#8220;Garofalo&#8221;</span>)
        </li>
        <li>
                          .Create();
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

Very well done but we still have a problem here. We are not giving a constraint to the developer that will use our fluent syntax. Let’s say that we are working with a retarded colleague, nobody can prohibit him to write something like this:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:8ad2de0a-3894-4005-96b1-b0aa15b56a6f" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Wrong Person
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">var</span> wrongPerson = <span style="color:#0000ff">new</span> <span style="color:#2b91af">PersonFactory</span>().Create();
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

In this case he will get a nice NullReferenceException because if he doesn’t call the method Initialize the factory won’t create a new instance of the class person … So how can we add a constraint to our interface? Very simple, we need 3 interfaces and not only one anymore. We need IInitFactory, IPersonFactory and ICreateFactory.

### Let’s see the code:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:d0703a9b-c3fd-4087-83e5-16420f144c82" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      IPersonFactory
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">interface</span> <span style="color:#2b91af">IPersonFactory</span>
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#2b91af">IPersonFactory</span> AddFirstName(<span style="color:#0000ff">string</span> firstName);
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#2b91af">IPersonFactory</span> AddLastName(<span style="color:#0000ff">string</span> lastName);
        </li>
        <li>
              <span style="color:#2b91af">IPersonFactory</span> AddMiddleName(<span style="color:#0000ff">string</span> middleName);
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#2b91af">IPerson</span> Create();
        </li>
        <li>
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

The IPersonFactory now will not be in charge anymore of creating a new instance of the class person, it will just be in charge of working with it. We will use dependency injection to **inject** a new instance. Let’s the concrete implementation of this factory:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:2a317a3f-8c84-479b-b651-e9047e8515f2" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Person Factory refactored
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">class</span> <span style="color:#2b91af">PersonFactory</span> : <span style="color:#2b91af">IPersonFactory</span>
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">private</span> <span style="color:#2b91af">IPerson</span> person = <span style="color:#0000ff">null</span>;
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">public</span> PersonFactory(<span style="color:#2b91af">IPerson</span> person)
        </li>
        <li>
              {
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#0000ff">this</span>.person = person;
        </li>
        <li>
              }
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
              <span style="color:#0000ff">public</span> <span style="color:#2b91af">IPersonFactory</span> AddFirstName(<span style="color:#0000ff">string</span> firstName)
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">this</span>.person.FirstName = firstName;
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#0000ff">return</span> <span style="color:#0000ff">this</span>;
        </li>
        <li>
              }
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
              <span style="color:#0000ff">public</span> <span style="color:#2b91af">IPersonFactory</span> AddLastName(<span style="color:#0000ff">string</span> lastName)
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">this</span>.person.LastName = lastName;
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#0000ff">return</span> <span style="color:#0000ff">this</span>;
        </li>
        <li>
              }
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
              <span style="color:#0000ff">public</span> <span style="color:#2b91af">IPersonFactory</span> AddMiddleName(<span style="color:#0000ff">string</span> middleName)
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">this</span>.person.MiddleName = middleName;
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#0000ff">return</span> <span style="color:#0000ff">this</span>;
        </li>
        <li>
              }
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
              <span style="color:#0000ff">public</span> <span style="color:#2b91af">IPerson</span> Create()
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">return</span> <span style="color:#0000ff">this</span>.person;
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

 

Now we need an **orchestrator**. Somebody that will be visible outside and will be in charge of giving to the fluent syntax a static flavor (we want to avoid the new Factory() syntax …) and that will return a PersonFactory ready to work …

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:9ef17660-75bb-4e23-b23c-4c03a6c0cbdf" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Person Fluent Factory
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">class</span> <span style="color:#2b91af">PersonFluentFactory</span>
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#2b91af">IPersonFactory</span> Init()
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">return</span> <span style="color:#0000ff">new</span> <span style="color:#2b91af">PersonFactory</span>(<span style="color:#0000ff">new</span> <span style="color:#2b91af">Person</span>());
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

 

And now Visual Studio will follow our rules …

<a href="http://raffaeu.com/wp-content/uploads/2013/03/14b1c3c3-2bcf-405c-99da-767de3f7abf2Parallels%20Picture_2.png" rel="lightbox[FluentInterface]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Parallels Picture" border="0" alt="Parallels Picture" src="http://raffaeu.com/wp-content/uploads/2013/03/9fe3ceea-9731-4b7e-b6d5-a61ed53e8c2fParallels%20Picture_thumb.png" width="260" height="103" /></a> 

<a href="http://raffaeu.com/wp-content/uploads/2013/03/ab1ef0d9-1cce-4309-8faa-aa0a2c4af2f6Parallels%20Picture%201_2.png" rel="lightbox[FluentInterface]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="Parallels Picture 1" border="0" alt="Parallels Picture 1" src="http://raffaeu.com/wp-content/uploads/2013/03/3197baab-2ad8-4a87-b445-d2996644370fParallels%20Picture%201_thumb.png" width="260" height="224" /></a> 

### Final Step, Lamba expression for a cool syntax.

Ok this is cool and it works like we want but … it is really time consuming. We want a fluent interface and that’s fine but if you a domain with 100 entities and more or less 100 factories, can you imagine the pain in the neck in order to adopt this pattern all over?? Well this is the reason you should study more in depth C#!! If you didn’t know, there is a cool syntax future in C# called Lambda Expressions. If you don’t know what I am talking about, <a href="http://msdn.microsoft.com/en-us/library/bb397687.aspx" target="_blank">have a look here</a>.

First of all we need a generic interface for our factory with only two methods, one to add a value to a property and one to return the current instance of the entity created by the factory.

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:ae66c133-6f69-407b-b6d8-a0ce858849a4" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      IGenericFactory
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">interface</span> <span style="color:#2b91af">IGenericFactory</span><T>
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#2b91af">IGenericFactory</span><T> AddPropertyValue(<span style="color:#2b91af">Expression</span><<span style="color:#2b91af">Func</span><T, <span style="color:#0000ff">object</span>>> property, <span style="color:#0000ff">object</span> value);
        </li>
        <li style="background: #f3f3f3">
              T Create();
        </li>
        <li>
          }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

Then following the same logic of the previous steps we need a concrete implementation but using generics and lambda expressions (I really love this part).

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:0ac42723-0dbd-425c-a106-252b371c4bee" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Generic Factory <T>
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">class</span> <span style="color:#2b91af">GenericFactory</span><T> : <span style="color:#2b91af">IGenericFactory</span><T>
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              T entity;
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
              <span style="color:#0000ff">public</span> GenericFactory(T entity)
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">this</span>.entity = entity;
        </li>
        <li style="background: #f3f3f3">
              }
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">public</span> <span style="color:#2b91af">IGenericFactory</span><T> AddPropertyValue(<span style="color:#2b91af">Expression</span><<span style="color:#2b91af">Func</span><T, <span style="color:#0000ff">object</span>>> property, <span style="color:#0000ff">object</span> value)
        </li>
        <li>
              {
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#2b91af">PropertyInfo</span> propertyInfo = <span style="color:#0000ff">null</span>;
        </li>
        <li>
                  <span style="color:#0000ff">if</span> (property.Body <span style="color:#0000ff">is</span> <span style="color:#2b91af">MemberExpression</span>)
        </li>
        <li style="background: #f3f3f3">
                  {
        </li>
        <li>
                      propertyInfo = (property.Body <span style="color:#0000ff">as</span> <span style="color:#2b91af">MemberExpression</span>).Member <span style="color:#0000ff">as</span> <span style="color:#2b91af">PropertyInfo</span>;
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
                      propertyInfo = (((<span style="color:#2b91af">UnaryExpression</span>)property.Body).Operand <span style="color:#0000ff">as</span> <span style="color:#2b91af">MemberExpression</span>).Member <span style="color:#0000ff">as</span> <span style="color:#2b91af">PropertyInfo</span>;
        </li>
        <li style="background: #f3f3f3">
                  }
        </li>
        <li>
                  propertyInfo.SetValue(entity, value, <span style="color:#0000ff">null</span>);
        </li>
        <li style="background: #f3f3f3">
           
        </li>
        <li>
                  <span style="color:#0000ff">return</span> <span style="color:#0000ff">this</span>;
        </li>
        <li style="background: #f3f3f3">
              }
        </li>
        <li>
           
        </li>
        <li style="background: #f3f3f3">
              <span style="color:#0000ff">public</span> T Create()
        </li>
        <li>
              {
        </li>
        <li style="background: #f3f3f3">
                  <span style="color:#0000ff">return</span> <span style="color:#0000ff">this</span>.entity;
        </li>
        <li>
              }
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

And the Fluent Factory, now generic, in this way:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:df8a202a-6e4a-4821-a674-59d5ccaadfe9" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Code Snippet
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#0000ff">class</span> <span style="color:#2b91af">GenericFluentFactory</span><T>
        </li>
        <li style="background: #f3f3f3">
          {
        </li>
        <li>
              <span style="color:#0000ff">public</span> <span style="color:#0000ff">static</span> <span style="color:#2b91af">IGenericFactory</span><T> Init(T entity)
        </li>
        <li style="background: #f3f3f3">
              {
        </li>
        <li>
                  <span style="color:#0000ff">return</span> <span style="color:#0000ff">new</span> <span style="color:#2b91af">GenericFactory</span><T>(entity);
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

 

The final syntax will be this one:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:d0482056-06c9-4dcc-a65f-5f5e5b3527d8" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      Code Snippet
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #ffffff; margin: 0 0 0 2em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#0000ff">var</span> person = <span style="color:#2b91af">GenericFluentFactory</span><<span style="color:#2b91af">Person</span>>
        </li>
        <li style="background: #f3f3f3">
              .Init(<span style="color:#0000ff">new</span> <span style="color:#2b91af">Person</span>())
        </li>
        <li>
              .AddPropertyValue(x => x.FirstName, <span style="color:#a31515">&#8220;Raffaele&#8221;</span>)
        </li>
        <li style="background: #f3f3f3">
              .AddPropertyValue(x => x.LastName, <span style="color:#a31515">&#8220;Garofalo&#8221;</span>)
        </li>
        <li>
              .Create();
        </li>
      </ol>
    </div></p>
  </div></p>
</div>

 

Ta da!! One generic factory with syntax constraints to create as many entities as you want! And the **smart** developer that is working with you won’t be able to complain at all.

### Final notes.

We can make this code better by:

  * Adding a custom type reflected by the property we are using, so you won’t be able to add an integer value to a property of type string and so on … 
  * Remove that ugly .Init(new Person()) using an IoC engine or a new constraint to our class, of course in this case you must provide classes with at least 1 parameters less constructor. 

That’s it! Have fun!

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:0767317B-992E-4b12-91E0-4F059A8CECA8:d222a47d-b7cc-41dc-8616-a2f24f9f2f66" class="wlWriterEditableSmartContent">
  Technorati Tags: <a href="http://technorati.com/tags/Fluent+Interface" rel="tag">Fluent Interface</a>,<a href="http://technorati.com/tags/Lambda+Expression" rel="tag">Lambda Expression</a>
</div>