---
id: 15
title: TDD a WCF service, is it possible?
date: 2013-01-29T14:58:55+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=15
permalink: /archive/2013/01/29/tdd-a-wcf-service-is-it-possible.aspx
categories:
  - TDD
  - WCF
---
Right now I am working on a <a href="http://martinfowler.com/bliki/CQRS.html" target="_blank">CQRS</a> project, and I am using WCF as my web service technology. In this project I decided to challenge myself and make a full TDD project. The first big problem I found was to simulate a real web server, because I didn’t want to hit a production or QA environment to run my tests.   
Second I didn’t want to write any integration test at all, so I had to figure out how to create an _in-Process_ test to run my WCF methods.

> _Why in-process test? When you write a unit test, you should test a single functionality, and in order to make the test atomic, it should run in-process. It should not involve third party tiers like a database or IIS, otherwise the test won’t be in-process anymore, so it will became an Integration test._

The first idea I came out was to fake the behaviour of IIS, and in order to accomplish this I could easily work with a mock framework like <a href="http://www.typemock.com/" target="_blank">Typemock</a> and mock out the entire IIS infrastructure.

Is this what I really want? Not at all. I don’t want and I don’t need to fake the behaviour of IIS, I need an instance of it because my WCF services are using custom factories and custom behaviours and I need to be sure they are acting in the right way.

So I have created two different type of tests. The first one is pure Unit Test, where I verify the command behaviours:

<div id="codeSnippetWrapper" style="overflow: auto; cursor: text; font-size: 8pt; border-top: silver 1px solid; font-family: 'Courier New', courier, monospace; border-right: silver 1px solid; border-bottom: silver 1px solid; padding-bottom: 4px; direction: ltr; text-align: left; padding-top: 4px; padding-left: 4px; margin: 20px 0px 10px; border-left: silver 1px solid; line-height: 12pt; padding-right: 4px; max-height: 200px; width: 97.5%; background-color: #f4f4f4">
  <div id="codeSnippet" style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4">
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum1" style="color: #606060">   1:</span> [TestMethod]</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum2" style="color: #606060">   2:</span> <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> AddUsersCommandHandler_ValidCommand_WillPersist()</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum3" style="color: #606060">   3:</span> {</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum4" style="color: #606060">   4:</span>     <span style="color: #008000">// arrange</span></pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum5" style="color: #606060">   5:</span>     AddUsersCommand expectedCommand = <span style="color: #0000ff">new</span> AddUsersCommand { UsersStream = expectedStream };</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum6" style="color: #606060">   6:</span>     <span style="color: #008000">// service with mocked unit of work</span></pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum7" style="color: #606060">   7:</span>     IServiceCommandHandler&lt;AddUsersCommand&gt; handler = <span style="color: #0000ff">new</span> AddUsersCommandHandler(mockUnit.Object);</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum8" style="color: #606060">   8:</span>  </pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum9" style="color: #606060">   9:</span>     <span style="color: #008000">// act</span></pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum10" style="color: #606060">  10:</span>     handler.Handle(expectedCommand);</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum11" style="color: #606060">  11:</span>  </pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum12" style="color: #606060">  12:</span>     <span style="color: #008000">// assertions against the command behaviour</span></pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum13" style="color: #606060">  13:</span>     mockUnit</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum14" style="color: #606060">  14:</span>         .Verify(x =&gt; x.BeginTransaction(), Times.Once());</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum15" style="color: #606060">  15:</span>     mockUnit</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum16" style="color: #606060">  16:</span>         .Verify(x =&gt; x.Save(It.IsAny&lt;User&gt;()));</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum17" style="color: #606060">  17:</span>     mockUnit</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum18" style="color: #606060">  18:</span>         .Verify(x =&gt; x.CommitTransaction(), Times.Once());</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum19" style="color: #606060">  19:</span> }</pre>
    
    <p>
      <!--CRLF--></div> </div> 
      
      <p>
        The second one is the (in)famous integration test, where you call the WCF service and test the overall process. Unfortunately for the second step I couldn’t find anything really TDD so I had to figure out by myself a different solution.
      </p>
      
      <h2>
        IIS Express
      </h2>
      
      <p>
        Yesterday I spent some time googling around for a solution while I found a nice <a href="http://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx" target="_blank">post from Scott Gu</a> about IIS express. With IIS express we have an in-process IIS server that we can startup or shutdown at any time during the lifecycle of our tests. It provides almost all the functionalities provided by IIS and it also trigger the <em>Application_Start</em> event, something not very well handled by Cassini …
      </p>
      
      <p>
        So, first of all I had to figure out how to bootstrap my unit tests to run over a different IIS than the one configured in the web.config.
      </p>
      
      <p>
        Piece of cake, first of all I need something that allows me to bootstrap IIS express before my unit test harness starts:
      </p>
      
      <div id="codeSnippetWrapper" style="overflow: auto; cursor: text; font-size: 8pt; border-top: silver 1px solid; height: 273px; font-family: 'Courier New', courier, monospace; border-right: silver 1px solid; border-bottom: silver 1px solid; padding-bottom: 4px; direction: ltr; text-align: left; padding-top: 4px; padding-left: 4px; margin: 20px 0px 10px; border-left: silver 1px solid; line-height: 12pt; padding-right: 4px; max-height: 200px; width: 97.5%; background-color: #f4f4f4">
        <div id="codeSnippet" style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4">
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum1" style="color: #606060">   1:</span> [ClassInitialize]</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum2" style="color: #606060">   2:</span> <span style="color: #0000ff">public</span> <span style="color: #0000ff">static</span> <span style="color: #0000ff">void</span> ClassInitialize(TestContext context)</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum3" style="color: #606060">   3:</span> {</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum4" style="color: #606060">   4:</span>     <span style="color: #008000">// create a process for IIS express</span></pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum5" style="color: #606060">   5:</span>     issProcess = <span style="color: #0000ff">new</span> Thread(IssUtility.StartIisExpress) </pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum6" style="color: #606060">   6:</span>                  { </pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum7" style="color: #606060">   7:</span>                     IsBackground = <span style="color: #0000ff">true</span> </pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum8" style="color: #606060">   8:</span>                  };</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum9" style="color: #606060">   9:</span>     <span style="color: #008000">// start it</span></pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum10" style="color: #606060">  10:</span>     issProcess.Start();</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum11" style="color: #606060">  11:</span>     <span style="color: #008000">// create a new channel factory using the generated URL</span></pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum12" style="color: #606060">  12:</span>     factory = <span style="color: #0000ff">new</span> ChannelFactory&lt;IDomainWriteService&gt;(</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum13" style="color: #606060">  13:</span>        <span style="color: #0000ff">new</span> BasicHttpBinding(), </pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum14" style="color: #606060">  14:</span>        <span style="color: #0000ff">new</span> EndpointAddress(<span style="color: #006080">"http://localhost:5555/services/DomainWriteService.svc"</span>));</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum15" style="color: #606060">  15:</span> }</pre>
          
          <p>
            <!--CRLF--></div> </div> 
            
            <p>
              Then I need another utility to kill everything when my test harness is done:
            </p>
            
            <div id="codeSnippetWrapper" style="overflow: auto; cursor: text; font-size: 8pt; border-top: silver 1px solid; font-family: 'Courier New', courier, monospace; border-right: silver 1px solid; border-bottom: silver 1px solid; padding-bottom: 4px; direction: ltr; text-align: left; padding-top: 4px; padding-left: 4px; margin: 20px 0px 10px; border-left: silver 1px solid; line-height: 12pt; padding-right: 4px; max-height: 200px; width: 97.5%; background-color: #f4f4f4">
              <div id="codeSnippet" style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4">
                <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum1" style="color: #606060">   1:</span> [ClassCleanup]</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum2" style="color: #606060">   2:</span> <span style="color: #0000ff">public</span> <span style="color: #0000ff">static</span> <span style="color: #0000ff">void</span> ClassCleanup()</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum3" style="color: #606060">   3:</span> {</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum4" style="color: #606060">   4:</span>     <span style="color: #008000">// close the WCF factory</span></pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum5" style="color: #606060">   5:</span>     factory.Close();</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum6" style="color: #606060">   6:</span>     <span style="color: #008000">// shutdown IIS express</span></pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum7" style="color: #606060">   7:</span>     IssUtility.TearDownIis();</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum8" style="color: #606060">   8:</span>     issProcess.Abort();</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum9" style="color: #606060">   9:</span> }</pre>
                
                <p>
                  <!--CRLF--></div> </div> 
                  
                  <p>
                    At this point I can write any test in the following format:
                  </p>
                  
                  <div id="codeSnippetWrapper" style="overflow: auto; cursor: text; font-size: 8pt; border-top: silver 1px solid; font-family: 'Courier New', courier, monospace; border-right: silver 1px solid; border-bottom: silver 1px solid; padding-bottom: 4px; direction: ltr; text-align: left; padding-top: 4px; padding-left: 4px; margin: 20px 0px 10px; border-left: silver 1px solid; line-height: 12pt; padding-right: 4px; max-height: 200px; width: 97.5%; background-color: #f4f4f4">
                    <div id="codeSnippet" style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4">
                      <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum1" style="color: #606060">   1:</span> [TestMethod]</pre>
                      
                      <p>
                        <!--CRLF-->
                      </p>
                      
                      <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum2" style="color: #606060">   2:</span> <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> ChannelFactory_ReceiveValidCommand_WillExecute()</pre>
                      
                      <p>
                        <!--CRLF-->
                      </p>
                      
                      <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum3" style="color: #606060">   3:</span> {</pre>
                      
                      <p>
                        <!--CRLF-->
                      </p>
                      
                      <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum4" style="color: #606060">   4:</span>     var expectedCommand = BuildAddUsersCommand();</pre>
                      
                      <p>
                        <!--CRLF-->
                      </p>
                      
                      <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum5" style="color: #606060">   5:</span>     service.AddUsers(expectedCommand);</pre>
                      
                      <p>
                        <!--CRLF-->
                      </p>
                      
                      <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum6" style="color: #606060">   6:</span> }</pre>
                      
                      <p>
                        <!--CRLF--></div> </div> 
                        
                        <p>
                          I found this technique very useful because we work with TFS on Azure so my Build environment cannot be polluted at all with any third party components or by installing the WCF service under test in the build machine.
                        </p>
                        
                        <p>
                          In the same way I found very quick to bootstrap IIS express for my tests, honestly they are faster than running on my local IIS 8.
                        </p>
                        
                        <h2>
                          IIS Express utility class
                        </h2>
                        
                        <p>
                          Below is the code I use to initialize IIS express, feel free to use it in your tests and let me know if you come up with a better solution than this one.
                        </p>
                        
                        <div id="codeSnippetWrapper" style="overflow: auto; cursor: text; font-size: 8pt; border-top: silver 1px solid; font-family: 'Courier New', courier, monospace; border-right: silver 1px solid; border-bottom: silver 1px solid; padding-bottom: 4px; direction: ltr; text-align: left; padding-top: 4px; padding-left: 4px; margin: 20px 0px 10px; border-left: silver 1px solid; line-height: 12pt; padding-right: 4px; max-height: 200px; width: 97.5%; background-color: #f4f4f4">
                          <div id="codeSnippet" style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4">
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum1" style="color: #606060">   1:</span> <span style="color: #0000ff">namespace</span> xxx</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum2" style="color: #606060">   2:</span> {</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum3" style="color: #606060">   3:</span>     <span style="color: #0000ff">using</span> System.Diagnostics;</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum4" style="color: #606060">   4:</span>  </pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum5" style="color: #606060">   5:</span>     <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> IssUtility</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum6" style="color: #606060">   6:</span>     {</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum7" style="color: #606060">   7:</span>         <span style="color: #0000ff">private</span> <span style="color: #0000ff">static</span> Process iisProcess;</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum8" style="color: #606060">   8:</span>         </pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum9" style="color: #606060">   9:</span>         <span style="color: #008000">// Start IIS on port 5555</span></pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum10" style="color: #606060">  10:</span>         <span style="color: #0000ff">public</span> <span style="color: #0000ff">static</span> <span style="color: #0000ff">void</span> StartIisExpress()</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum11" style="color: #606060">  11:</span>         {</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum12" style="color: #606060">  12:</span>             var startInfo = <span style="color: #0000ff">new</span> ProcessStartInfo</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum13" style="color: #606060">  13:</span>                 {</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum14" style="color: #606060">  14:</span>                     WindowStyle = ProcessWindowStyle.Normal,</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum15" style="color: #606060">  15:</span>                     ErrorDialog = <span style="color: #0000ff">true</span>,</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum16" style="color: #606060">  16:</span>                     LoadUserProfile = <span style="color: #0000ff">true</span>,</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum17" style="color: #606060">  17:</span>                     CreateNoWindow = <span style="color: #0000ff">false</span>,</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum18" style="color: #606060">  18:</span>                     UseShellExecute = <span style="color: #0000ff">false</span>,</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum19" style="color: #606060">  19:</span>                     Arguments =</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum20" style="color: #606060">  20:</span>                         <span style="color: #0000ff">string</span>.Format(<span style="color: #006080">"/path:\"{0}\" /port:{1}"</span>,</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum21" style="color: #606060">  21:</span>                                         <span style="color: #006080">@"your WCF dev path"</span>, 5555)</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum22" style="color: #606060">  22:</span>                 };</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum23" style="color: #606060">  23:</span>  </pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum24" style="color: #606060">  24:</span>             var programfiles = <span style="color: #0000ff">string</span>.IsNullOrEmpty(startInfo.EnvironmentVariables[<span style="color: #006080">"programfiles"</span>])</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum25" style="color: #606060">  25:</span>                                     ? startInfo.EnvironmentVariables[<span style="color: #006080">"programfiles(x86)"</span>]</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum26" style="color: #606060">  26:</span>                                     : startInfo.EnvironmentVariables[<span style="color: #006080">"programfiles"</span>];</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum27" style="color: #606060">  27:</span>  </pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum28" style="color: #606060">  28:</span>             startInfo.FileName = programfiles + <span style="color: #006080">"\\IIS Express\\iisexpress.exe"</span>;</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum29" style="color: #606060">  29:</span>  </pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum30" style="color: #606060">  30:</span>             <span style="color: #0000ff">try</span></pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum31" style="color: #606060">  31:</span>             {</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum32" style="color: #606060">  32:</span>                 iisProcess = <span style="color: #0000ff">new</span> Process {StartInfo = startInfo};</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum33" style="color: #606060">  33:</span>  </pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum34" style="color: #606060">  34:</span>                 iisProcess.Start();</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum35" style="color: #606060">  35:</span>                 iisProcess.WaitForExit();</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum36" style="color: #606060">  36:</span>             }</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum37" style="color: #606060">  37:</span>             <span style="color: #0000ff">catch</span></pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum38" style="color: #606060">  38:</span>             {</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum39" style="color: #606060">  39:</span>                 iisProcess.CloseMainWindow();</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum40" style="color: #606060">  40:</span>                 iisProcess.Dispose();</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum41" style="color: #606060">  41:</span>             }</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum42" style="color: #606060">  42:</span>         }</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum43" style="color: #606060">  43:</span>  </pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum44" style="color: #606060">  44:</span>         <span style="color: #008000">// kill everything</span></pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum45" style="color: #606060">  45:</span>         <span style="color: #0000ff">public</span> <span style="color: #0000ff">static</span> <span style="color: #0000ff">void</span> TearDownIis()</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum46" style="color: #606060">  46:</span>         {</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum47" style="color: #606060">  47:</span>             iisProcess.CloseMainWindow();</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum48" style="color: #606060">  48:</span>             iisProcess.Dispose();</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum49" style="color: #606060">  49:</span>         }</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum50" style="color: #606060">  50:</span>     }</pre>
                            
                            <p>
                              <!--CRLF-->
                            </p>
                            
                            <pre style="border-top-style: none; overflow: visible; font-size: 8pt; border-left-style: none; font-family: 'Courier New', courier, monospace; border-bottom-style: none; color: black; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; line-height: 12pt; padding-right: 0px; width: 100%; background-color: #f4f4f4"><span id="lnum51" style="color: #606060">  51:</span> }</pre>
                            
                            <p>
                              <!--CRLF--></div> </div> 
                              
                              <p>
                                Enjoy!
                              </p>