---
id: 507
title: Where is the Magic wand?
date: 2013-07-27T14:23:26+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=507
permalink: /archive/:year/:month/:day/:title/
categories:
  - Agile
tags:
  - agile
---
<img src="http://blog.raffaeu.com/wp-content/uploads/2013/07/Smiling_Magic_Wand.jpg" alt="Smiling Magic Wand" title="Smiling_Magic_Wand.jpg" border="0" width="96" height="96" style="float:left;" />

I am always wondering where I was the day they distributed the Magic Wand. I mean, every time I join a new Team/Project I always have to carry with me a set of tools that make my job easier. Some of these tools are within Visual Studio, some are pre-defined architecture diagrams that I created with Visio or Balsamiq, some are links to articles and books that I share with the teams. 

Unfortunately, very often I see the demand of a Magic Wand, a tool that I don&#8217;t carry with me just because I simply **don&#8217;t have** it and probably I&#8217;ll never have!

Now let&#8217;s make the concept more clear, there are usually three different situations where you will be required the use of a Magic Wand:

  1. First, most common, **the impossible timeline**. You get a request from a Stakeholder or even a Product Owner to accomplish a task in a time that it&#8217;s human impossible
  2. Second **the Ferrari buyer**, you are required to design a functionality or a set of functionalities with a budget that is waaaaay to low than the minimum required
  3. Third **the Tetris puzzle**, you are required to add a functionality to an existing structure, but the existing structure does not allow you to implement the functionality and you don&#8217;t have time/resources/space to refactor the existing code

Of course there are a lot more situations where you are required to provide a Magic Wand, the three reasons mentioned previously are common in my job, and this is how I usually try to tackle them, _even if this doesn&#8217;t mean that my solution is always the right one &#8230;_

### The impossible timeline

<img src="http://blog.raffaeu.com/wp-content/uploads/2013/07/Screen-Shot-2013-07-18-at-11.35.04-AM.png" alt="Screen Shot 2013 07 18 at 11 35 04 AM" title="Screen Shot 2013-07-18 at 11.35.04 AM.png" border="1 px solid gray" margin="3px 3px 3px 3px" padding="3px 3px 3px 3px" width="45%" height="45%" style="float:left;" />

You have a meeting with your Product Owner and you discover right away that he wants you to implement a very nice piece of functionality. It requires some refactoring of the current code, a bit of investigation on your side, and probably a couple of weeks between coding/testing and update the documentation. Wow, great, you know that you&#8217;ll be busy in the next months with something very cool so you are all trilled and start to discuss with the PO a draft of a backlog that you have created previously.

Right away you discover that your backlog is simply non achievable. You estimated three sprints for a total of almost two months of works while your PO has already said to the Stakeholders that it won&#8217;t take more than a couple of weeks!

In this case, the only thing that we can do is to draw the backlog, probably using a Story Map approach, and share the backlog with Stakeholders and POs together, in order to show what is the real amount of work required. I usually work with Balsamiq and I create story maps that look like the one on your left. 

Using this approach you can clearly show to the Stakeholders that in order to make an Order, for example, you need to create few things like: infrastructure, HTML views, REST methods and so on. For each task you can clearly identify how long it will take and that will probably give to them a better picture of what&#8217;s needed to be done.

### The Ferrari buyer

The second situation where I am usually forced to use a Magic Wand is when I encounter the Ferrari buyers. Why do I call them Ferrari buyer? Well because those type of Customers/Stakeholders or whatever you want to call them, they are usually looking for a Ferrari master piece but with the budget of an old Fiat. That&#8217;s why they struggle to find a &#8220;YES&#8221; answer when they propose the project or they request a new functionality. 

Did it ever happen to you? You propose a project for a budget, let&#8217;s say of 30K a month, the Stakeholders are trilled and excited, every body approve your project, but usually for a third of the budget … Wow, how can we fix this now? They want the functionality, they want us to implement it but with half of the planned resources &#8230;

In this case is not enough to show to your customers what are the steps required for a task, you need to start to talk also about resources and time. If you can prove how long it takes a piece of functionality and how much will cost the person involved in the project, you can then easily come up with a formula like this one: 

#### Cost = Time * DeveloperCostPerHour

And I usually implement this concept on top of my backlog, like the following picture: 

<img src="http://blog.raffaeu.com/wp-content/uploads/2013/07/Screen-Shot-2013-07-25-at-2.05.31-PM.png" alt="Screen Shot 2013 07 25 at 2 05 31 PM" title="Screen Shot 2013-07-25 at 2.05.31 PM.png" border="0" width="600" height="272" />

Ok this is not a Magic Wand but at least you can show to the **Ferrari Buyer** that the optionals are not coming for free! 

### The Tetris Puzzle

Ok, this one has happened to all of us at least once, I can&#8217;t believe you never had to code a new functionality into an existing mess (ops), into an existing application, with some crazy acceptance criteria. 

Let&#8217;s make an example. A while ago I had to work with an existing Windows Form + C# platform, used to generate mathematical results after the execution of a long running task. The long running task was executed outside the context of the app, it was on a different tier (Database tier).    
The application had a major issue, the entire User iInterface was synchronous, so every time a request was made (Data, Commands), the User had to wait until the User Interface was free.
  
_You would say, what a pain …_  
Btw, the request from the Product Owner was to make the User Interface asynchronous, using less code as possible, in the minimum achievable and high quality level possible amount of time. 

We analysed the application and we discovered that there were a lot of Views, User Controls and customisation from third party libraries (_like Telerik, DevExpress …_) that required a complete refactor process because they were not able to receive asynchronous data properly, without raising **Invalid thread exceptions** here and there.

Well, at the end it was hard to convince the PO about a massive refactor, but we didn&#8217;t really gave him a second choice, and this is really the point. If you give them a second cheapest choice, **they will always choose that one** and you will be stuck in the middle not able to say NO and not able to finish in time.

Well I hope you enjoyed my _rant_ …
  
  
Raffaeu