---
id: 16
title: Agile Architecture.
date: 2012-05-15T09:24:14+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=16
permalink: /archive/2012/05/15/agile-architecture.aspx
categories:
  - Design Pattern
  - TDD
  - TypeMock
---
Last week I presented a webinar about Agile Architecture and I got an unexpected positive feedback from it. First it was unexpected because I didn’t expect to get lot of people interested in this particular topic. Second it was unexpected because I am still at the beginning of my teaching career and presenting webinars so I am not so good yet to keep the audience interested for 60 min.

Btw, considering the result of the webinar, I decided to post the video of the webinar (thanks to the guys of <a href="http://www.typemock.com/" target="_blank">Typemock</a>!) and a short introduction to <a href="http://www.agilearchitect.org/" target="_blank">Agile Architecture</a>.

#### Agile Architecture webinar:

<div style="border-top: #cccccc 1px solid; border-right: #cccccc 1px solid; border-bottom: #cccccc 1px solid; margin: 2px; border-left: #cccccc 1px solid">
</div>

<h4 style="Border: 1px solid #CCCCCC; Padding:2px;Background: #FFFF66">
  Update: slides available on SlideShare.
</h4>

<div style="width:425px" id="__ss_12951051">
  <strong style="display:block;margin:12px 0 4px"><a href="http://www.slideshare.net/raffaeu/software-architecture-in-an-agile-environment" title="Software architecture in an agile environment" target="_blank">Software architecture in an agile environment</a></strong> </p> 
  
  <div style="padding:5px 0 12px">
    View more <a href="http://www.slideshare.net/" target="_blank">presentations</a> from <a href="http://www.slideshare.net/raffaeu" target="_blank">Raffaele Garofalo</a>
  </div></p>
</div>

### Agile architecture

What is Agile Architecture? It is a methodology that follows the Agile guidelines but differ from some aspect of its process. 

At this web address: [http://www.agilearchitect.org/](http://www.agilearchitect.org/ "http://www.agilearchitect.org/") you can find _probably_ the official website of this Agile methodology, even if you may find a lot of interesting discussions and articles here and there on the web, just <a href="http://www.google.com/#hl=en&gs_nf=1&cp=13&gs_id=1d&xhr=t&q=agile+architecture&pf=p&safe=off&output=search&sclient=psy-ab&oq=agile+archite&aq=0&aqi=g4&aql=&gs_l=&pbx=1&bav=on.2,or.r_gc.r_pw.r_qf.,cf.osb&fp=12ac40e9db346b04&biw=1280&bih=647" target="_blank">google “Agile Architecture”</a>. 

Another interesting web site is <http://www.agilemodeling.com/> where you can find tons of information about Agile Modeling and Agile Architecture. (_If you watch the webinar I spoke about both of them_).

Finally, if you want just to read a quick article and understand the overall process of Agile Architecture, I found this article of <a href="http://shapingsoftware.com/about/" target="_blank">J.D. Meier</a> very helpful: “<a href="http://apparch.codeplex.com/wikipage?title=How%20To%20-%20Design%20Using%20Agile%20Architecture" target="_blank">How to design with Agile Architecture</a>”.

### TFS Template

I got various requests about the TFS and Dashboard templates I used during the webinar. I got them from the beta of Team Foundation Server 2011, available <a href="http://msdn.microsoft.com/en-us/vstudio/ff637362" target="_blank">here</a>. You need to customize the process to fit better Agile Architecture methodology. I am planning to prepare a custom template for TFS 2011 as soon as the SDK will be finalized. 

### Q/A from the webinar

Below is a short list of some of the most interesting Questions and Answers I got during the webinar. I am posting them here because during the webinar I wasn’t able to read them because I am using the MAC version of GoToWebinar.

**Q: Acceptance Tests should have been written before the model.**

_Not really, if you adopt the strict TDD all the acceptance tests should be written before, but when you envision the architecture, it is too early to write acceptance   
tests. When you start the iteration and you have your requirements defined, you may start to model against the acceptance criteria for that iteration. But remember   
that the modeling part should be light enough to provide value for the next step._

**Q: the functions of the architect are clear, but it is not clear how these functions are intended to fit into the Sprints.**

_The sprint is composed by three parts, modeling, brainstorm and coding. The architect will perfectly fit into each of this phase. I think I have explained this   
concept during the webinar, but just to be sure. During modeling and brainstorm the architect should help with his knowledge and collaborate in the modeling, during   
the coding he should be an active coder too and contribute to the technical decisions providing knowledge and expertise._

**Q: Can you elaborate on what you mean with the &#8220;design phase&#8221;?**

_Ok, during the first iteration, the envision, there is a light design phase. You should design some scratches of your envision, representing the architecture and the UI, but you should not invest too much time.   
Design phase while modeling means adopting UML to design your models._

**Q: How does your &#8220;Iter: 0&#8221; relate to the PO, backlog and sprint planning?**

_Ah ah, interesting. The envision or iteration 0 is tricky to fit into SCRUM. Take the classic meter to measure an iteration, like 1 or 2 weeks. Inside this timeframe you should have enough space and time to prepare the necessary analysis and mocks required by next steps. So you may consider iter 0 like a beginning sprint._ 

**Q: Couldn&#8217;t the use-case diagram be enough for the modelling phase? I&#8217;m interested in hearing your reasoning because I&#8217;m currently in this situation in my current project. What level of detail is enough when it comes to architecture that lets developers use their brain and problem solving power?**

_No, the use case is not enough because it may incur into personal interpretation. With the use case and the acceptance you and your team are ready for the modeling phase, where you create the architectural style for that feature. The modeling should be enough to represent the current feature and it should not be implemented, the implementation occurs in the next phase where you apply TDD against the model discussed in the previous part. It is hard to keep the model the blueprint of the code and often it doesn&#8217;t provide value. It is important that the model represents what you are delivering._

**Q: Does the Process workflow you describe impact the philosophy of embracing change? How can we perform radical change when we get a bigger up-front model?** 

_No, it doesn&#8217;t because Agile Architecture pretends the flexibility into your model. Model just enough to represents the feature and remember that you can always come back and re-model and re-factor. Embracing Agile Architecture will force you to have a dynamic more agnostic and flexible model than using a classic architecture approach.   
Why would you get a radical change request with a radical model upfront? Of you adopt Agile you should structure these changes into small iterations._

_The envision phase is the most critical, you or the architect need to understand what the stakeholder wants from your team. If you get that, you are in a good starting position.   
The big mistake is to take too much for the envision, which is not the modeling phase. Mocking out your application doesn&#8217;t mean to model everything, so the envision should take few days, at most one week. If it takes more you may have some smells in your team: lack of requirements, lack of business knowledge, lack of technical knowledge.   
Remember that every iteration has a modeling phase and a brainstorm phase, so you may move back to the envision and adapt it to the new requirements._ 

<div id="scid:0767317B-992E-4b12-91E0-4F059A8CECA8:347fbe6a-85f9-4232-a2b1-1b59e51b5f50" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px">
  Technorati Tags: <a href="http://technorati.com/tags/Agile+Architecture" rel="tag">Agile Architecture</a>,<a href="http://technorati.com/tags/Agile" rel="tag">Agile</a>,<a href="http://technorati.com/tags/Scrum" rel="tag">Scrum</a>,<a href="http://technorati.com/tags/Agile+Modeling" rel="tag">Agile Modeling</a>,<a href="http://technorati.com/tags/Typemock" rel="tag">Typemock</a>,<a href="http://technorati.com/tags/Webinar" rel="tag">Webinar</a>
</div>