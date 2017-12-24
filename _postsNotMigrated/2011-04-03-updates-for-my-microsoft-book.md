---
id: 130
title: Updates for my Microsoft Book
date: 2011-04-03T16:53:49+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=130
permalink: /archive/2011/04/03/updates-for-my-microsoft-book.aspx
categories:
  - 'C#'
  - Design Pattern
  - MVVM
  - Silverlight
  - WPF
---
After one week that my book about LOB applications has been published I started to receive some additional questions that I am trying to address in this post.

First of all I want to thank all the guys and girls that are buying the book and all the people that are providing feedbacks for the book. I really appreciate. I want also to specify that the Microsoft book has not been released as a “Book about MVVM” but more as a “book to discover LOB and layered applications”. I believe that part of the misunderstanding as been caused by the book’s title but we wanted to specify the MVVM keyword in the title because the book spent two chapters on it.

**The source code** has been published and you can find in the book the correct address that will point you to the download. I want to thanks Ted Anderson that on Amazon.com has notified me about the error done by OReilly in the publishing address. The source code is available here: 

<http://examples.oreilly.com/9780735650923-files/­9780735650923_files.zip>. Please do not download the code and pretend to get explanations from my if you didn’t buy the book yet … <img style="border-bottom-style: none; border-right-style: none; border-top-style: none; border-left-style: none" class="wlEmoticon wlEmoticon-winkingsmile" alt="Winking smile" src="http://raffaeu.com/wp-content/uploads/2013/03/e53a69e2-7755-4708-adef-65827d0797efwlEmoticon-winkingsmile_2.png" />

**The T-SQL** to generate the database is not necessary. When you will open the Visual Studio 2010 solution, you need to run the Test show here:

<div style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:afc029b9-ad04-4165-8ed7-d9f52d271b5e" class="wlWriterEditableSmartContent">
  <div style="border: #000080 1px solid; color: #000; font-family: 'Courier New', Courier, Monospace; font-size: 10pt">
    <div style="background: #000080; color: #fff; font-family: Verdana, Tahoma, Arial, sans-serif; font-weight: bold; padding: 2px 5px">
      TDD for generate the SQL Datab
    </div>
    
    <div style="background: #ddd; max-height: 300px; overflow: auto">
      <ol style="background: #000000; margin: 0 0 0 2.5em; padding: 0 0 0 5px;">
        <li>
          <span style="color:#ffffff">[</span><span style="color:#ffc66d">TestFixtureSetUp</span><span style="color:#ffffff">]</span>
        </li>
        <li style="background: #0c0c0c">
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> CanCreateDatabaseSchema()</span>
        </li>
        <li>
          <span style="color:#ffffff">{</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#cc7832">try</span>
        </li>
        <li>
              <span style="color:#ffffff">{</span>
        </li>
        <li style="background: #0c0c0c">
                  <span style="color:#ffffff"></span><span style="color:#cc7832">var</span><span style="color:#ffffff"> cfg = </span><span style="color:#cc7832">new</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">Configuration</span><span style="color:#ffffff">();</span>
        </li>
        <li>
                  <span style="color:#ffffff">cfg.Configure();</span>
        </li>
        <li style="background: #0c0c0c">
                  <span style="color:#ffffff">cfg.AddAssembly(</span><span style="color:#ffc66d">Assembly</span><span style="color:#ffffff">.Load(</span><span style="color:#a5c25c">&#8220;CRM.Dal.Nhibernate&#8221;</span><span style="color:#ffffff">));</span>
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#cc7832">new</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">SchemaExport</span><span style="color:#ffffff">(cfg).Execute(</span><span style="color:#cc7832">true</span><span style="color:#ffffff">, </span><span style="color:#cc7832">true</span><span style="color:#ffffff">, </span><span style="color:#cc7832">false</span><span style="color:#ffffff">);</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">}</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#cc7832">catch</span><span style="color:#ffffff"> (</span><span style="color:#ffc66d">Exception</span><span style="color:#ffffff"> exception)</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">{</span>
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.Fail(exception.ToString());</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">}</span>
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
          <span style="color:#ffffff"></span><span style="color:#cc7832">public</span><span style="color:#ffffff"> </span><span style="color:#cc7832">void</span><span style="color:#ffffff"> CanGetAUnitOfWork()</span>
        </li>
        <li>
          <span style="color:#ffffff">{</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff"></span><span style="color:#cc7832">try</span>
        </li>
        <li>
              <span style="color:#ffffff">{</span>
        </li>
        <li style="background: #0c0c0c">
                  <span style="color:#ffffff"></span><span style="color:#6897bb">ISessionFactory</span><span style="color:#ffffff"> factory = </span><span style="color:#cc7832">new</span><span style="color:#ffffff"> </span><span style="color:#ffc66d">SessionFactory</span><span style="color:#ffffff">();</span>
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#6897bb">IUnitOfWork</span><span style="color:#ffffff"> unitOfWork = factory.CurrentUoW;</span>
        </li>
        <li style="background: #0c0c0c">
                  <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(unitOfWork, </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.Not.Null);</span>
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.That(unitOfWork.Orm, </span><span style="color:#ffc66d">Is</span><span style="color:#ffffff">.Not.Null);</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">}</span>
        </li>
        <li>
              <span style="color:#ffffff"></span><span style="color:#cc7832">catch</span><span style="color:#ffffff"> (</span><span style="color:#ffc66d">Exception</span><span style="color:#ffffff"> exception)</span>
        </li>
        <li style="background: #0c0c0c">
              <span style="color:#ffffff">{</span>
        </li>
        <li>
                  <span style="color:#ffffff"></span><span style="color:#ffc66d">Assert</span><span style="color:#ffffff">.Fail(exception.ToString());</span>
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

 

<font face="Segoe UI"><font color="#000000"><strong>REMEMBER THAT BEFORE RUNNING THE TEST YOU SHOULD CREATE THE DATABASE IN SQL SERVER AND CHANGE THE CONNECTION STRING IN THE APP.CONFIG</strong>.</font></font>

<font color="#000000" face="Segoe UI">Also remember that the source code has been created only to show you some practical examples of how to layer a LOB application, how to use NHibernate or Entity Framework with the same data layer and how to architect the MVVM pattern. If you want to use the application in a production environment, you have to spent additional time on it in order to get a <strong>final product</strong>.</font>

<font color="#000000" face="Segoe UI">For any other additional information I am here.</font>

<font color="#000000" face="Segoe UI">Please if you find errors or mistakes in the book, I would really appreciate if you can post an <font size="2">errata corrigge in the corresponding section of the OReilly web site: </font></font><a style="line-height: normal" href="http://oreilly.com/catalog/0790145309686/"><u><font color="#000000" size="2" face="Segoe UI">http://oreilly.com/catalog/0790145309686/</font></u></a> <font color="#000000" size="2" face="Segoe UI"></font>

<font color="#000000" size="3" face="Segoe UI">Next month I will publish my second book “Applied WPF in Context” with APRESS; in that book you will find <strong>whatever</strong> you need to learn WPF and the MVVM pattern. </font>

<font color="#000000" face="Segoe UI">Stay tuned! </font>