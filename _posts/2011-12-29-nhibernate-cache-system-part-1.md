---
id: 58
title: NHibernate cache system. Part 1
date: 2011-12-29T20:22:49+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=58
permalink: /archive/2011/12/29/nhibernate-cache-system-part-1.aspx
categories:
  - 'C#'
  - NHibernate
---
In this series of articles I will try to explain you how NHibernate cache system works and how it should be used in order to get the best performance/configuration from this product.

  * <a href="#" target="_blank">Introduction</a>  
      * Architecture 
      * Cache Level 1 mechanism 
      * Load and Get, what’s the difference 
      * Session maintenance: Evict, Flush and Clear 
  * <a href="http://blog.raffaeu.com/archive/2011/12/31/nhibernate-cache-system-part-2.aspx" target="_blank">2nd Level Cache</a> 
      * Architecture 
      * Configuration
      * Cache by mapping
      * Cache by query
      * Final advice
  * <a href="http://blog.raffaeu.com/archive/2011/12/29/nhibernate-cache-system-part-3.aspx" target="_blank">Cache providers (most known)</a>

### NHibernate Cache architecture

NHibernate has an internal cache architecture that I will define **absolutely well done**. On an architectural point of view, it is designed for the enterprise and it is 100% configurable. Consider that it allows you to create also your custom cache provider!

The following picture show the cache architecture overview of NHibernate (actually the version I am talking about is the 3.2 GA).

[<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/9333692a-0d14-40dd-bcbc-fc5ce4a9e01eimage_thumb.png" width="260" height="221" />](http://raffaeu.com/wp-content/uploads/2013/03/6d7183df-843c-45a9-9dfb-c3ee5fe2b94eimage_2.png)

The cache system is composed by two **levels**, the cache of level 1 that _usually_ it is configured by default if you are working with the _ISession_ object, and the cache of level 2 that by default **is disabled**.

The cache of level 1 is provided by the ISession data context and it is maintained by the lifecycle of the _ISession_ object, this means that as soon as you destroy (dispose) an _ISession_ object, also the cache of level 1 will be destroyed and all the corresponding objects will be detached from the ISession. This cache system works on a per transaction basis and it is designed to reduce the number of database calls during the lifecycle of an _ISession_. As an example, you should use this cache if you have the need to access and modify an object in a transaction, multiple times.

The cache of level 2 is provided by the _ISessionFactory_ component and it is shared across **all the session created using the same factory**. Its lifecycle correspond to the lifecycle of the session factory and it provides a more powerful **but also dangerous** set of features. It allows you to keep objects in cache across multiple transactions and sessions; the objects are available **everywhere** and not only on the client that is using a specific _ISession_.  

### Cache Level 1 mechanism

As soon as you start to create a new _ISession_ (not an _IStatelessSession!!_) NHibernate starts to holds in memory, using a specific mechanism, all the objects that are involved with the current session. The methods used by NHibernate to load the data into the cache are two: **Get<T>(id)** and **Load<T>(id)**. This means that if you try to load one or more entities using: LinQ, HQL, ICriteria … NHibernate **will not put them into the cache of level 1**.

Another way to put an object into the lvl1 cache is to use persistence methods like Save, Delete, Update and SaveOrUpdate. 

[<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/e57d1c34-00a9-4918-9262-04729cc6ba06image_thumb_1.png" width="251" height="260" />](http://raffaeu.com/wp-content/uploads/2013/03/0dc3762d-9ba3-4f45-b0d7-f0c7b9e269c9image_4.png)

As you can see from the previous picture, the _ISession_ object is able to contains two different categories of entities, the one that we define “loaded” using Get or Load and the one that we define “dirty”, which means that they were somehow modified and associated with a session.

### Load and Get, what’s the difference?

A major confusion I personally noticed while working with NHibernate is the not correct usage of the two methods Get and Load so let’s see for a moment how they work and when they should or should not be used.

<table border="1" cellspacing="0" cellpadding="2" width="547">
  <tr>
    <td valign="top" width="88">
      <p align="left">
        </td> 
        
        <td valign="top" width="181">
          <h4 align="left">
            Get
          </h4>
        </td>
        
        <td valign="top" width="11">
          <p align="left">
            </td> 
            
            <td valign="top" width="254">
              <h4 align="left">
                Load
              </h4>
            </td>
            
            <td valign="top" width="11">
               
            </td></tr> 
            
            <tr>
              <td valign="top" width="93">
                <h4 align="left">
                  Fetch method
                </h4>
              </td>
              
              <td valign="top" width="190">
                <p align="left">
                  Retrieve the entire entity in one SELECT statement and puts the entity in the cache.
                </p>
              </td>
              
              <td valign="top" width="11">
                <p align="left">
                  </td> 
                  
                  <td valign="top" width="262">
                    <p align="left">
                      Retrieve only the ID of the entity and returns a non fetched proxy instance of the entity. As soon as you “hit” a property, the entity is loaded.
                    </p>
                  </td>
                  
                  <td valign="top" width="11">
                    <p align="left">
                      </td> </tr> 
                      
                      <tr>
                        <td valign="top" width="94">
                          <h4 align="left">
                            How it loads
                          </h4>
                        </td>
                        
                        <td valign="top" width="193">
                          <p align="left">
                            It verifies if the entity is in the cache, otherwise it tries to execute a SELECT
                          </p>
                        </td>
                        
                        <td valign="top" width="11">
                          <p align="left">
                            </td> 
                            
                            <td valign="top" width="264">
                              <p align="left">
                                It verifies if the entity is in the cache, otherwise it tries to execute a SELECT
                              </p>
                            </td>
                            
                            <td valign="top" width="11">
                              <p align="left">
                                </td> </tr> 
                                
                                <tr>
                                  <td valign="top" width="94">
                                    <h4 align="left">
                                      Not Available
                                    </h4>
                                  </td>
                                  
                                  <td valign="top" width="194">
                                    <p align="left">
                                      If the entity does not exist, it returns NULL
                                    </p>
                                  </td>
                                  
                                  <td valign="top" width="11">
                                    <p align="left">
                                      </td> 
                                      
                                      <td valign="top" width="265">
                                        <p align="left">
                                          If the entity does not exist, it THROW an exception
                                        </p>
                                      </td>
                                      
                                      <td valign="top" width="11">
                                        <p align="left">
                                          </td> </tr> 
                                          
                                          <tr>
                                            <td valign="top" width="94">
                                              <p align="left">
                                                </td> 
                                                
                                                <td valign="top" width="194">
                                                  <p align="left">
                                                    </td> 
                                                    
                                                    <td valign="top" width="11">
                                                      <p align="left">
                                                        </td> 
                                                        
                                                        <td valign="top" width="265">
                                                          <p align="left">
                                                            </td> 
                                                            
                                                            <td valign="top" width="11">
                                                               
                                                            </td></tr> </tbody> </table> 
                                                            
                                                            <p>
                                                              I personally prefer Get because it returns NULL instead of throwing a nasty exception, but this is a personal choice; while some of you may prefer to use Load because you want to avoid a database call until is really needed.
                                                            </p>
                                                            
                                                            <p>
                                                              Below I wrote a couple of very simple tests to show you how the Get and Load methods work across the same <em>ISession</em>.
                                                            </p>
                                                            
                                                            <h3>
                                                              Get<T>()
                                                            </h3>
                                                            
                                                            <div class="csharpcode">
                                                              <pre class="alt"><span class="lnum">   1:  </span>[Test]</pre>
                                                              
                                                              <pre><span class="lnum">   2:  </span>[Category(<span class="str">"Database"</span>)]</pre>
                                                              
                                                              <pre class="alt"><span class="lnum">   3:  </span><span class="kwrd">public</span> <span class="kwrd">void</span> UsingGetThePersonIsFullyCached()</pre>
                                                              
                                                              <pre><span class="lnum">   4:  </span>{</pre>
                                                              
                                                              <pre class="alt"><span class="lnum">   5:  </span>    <span class="kwrd">using</span> (var session = factory.OpenSession())</pre>
                                                              
                                                              <pre><span class="lnum">   6:  </span>    {</pre>
                                                              
                                                              <pre class="alt"><span class="lnum">   7:  </span>        <span class="kwrd">using</span> (var tx = session.BeginTransaction(IsolationLevel.ReadCommitted))</pre>
                                                              
                                                              <pre><span class="lnum">   8:  </span>        {</pre>
                                                              
                                                              <pre class="alt"><span class="lnum">   9:  </span>            var persons = MockFactory.MakePersons();</pre>
                                                              
                                                              <pre><span class="lnum">  10:  </span>            persons.ForEach(p =&gt; session.Save(p));</pre>
                                                              
                                                              <pre class="alt"><span class="lnum">  11:  </span>            tx.Commit();</pre>
                                                              
                                                              <pre><span class="lnum">  12:  </span>            session.Clear();</pre>
                                                              
                                                              <pre class="alt"><span class="lnum">  13:  </span>            Console.WriteLine(<span class="str">"*** FIRST SELECT ***"</span>);</pre>
                                                              
                                                              <pre><span class="lnum">  14:  </span>            var expectedPerson1 = session.Get&lt;Person&gt;(persons[0].PersonId);</pre>
                                                              
                                                              <pre class="alt"><span class="lnum">  15:  </span>            Assert.That(expectedPerson1, Is.Not.Null);</pre>
                                                              
                                                              <pre><span class="lnum">  16:  </span>            Console.WriteLine(<span class="str">"*** SECOND SELECT ***"</span>);</pre>
                                                              
                                                              <pre class="alt"><span class="lnum">  17:  </span>            var expectedPerson2 = session.Get&lt;Person&gt;(persons[0].PersonId);</pre>
                                                              
                                                              <pre><span class="lnum">  18:  </span>            Assert.That(expectedPerson2, Is.Not.Null);</pre>
                                                              
                                                              <pre class="alt"><span class="lnum">  19:  </span>            Assert.That(expectedPerson2.FirstName, Is.Not.EqualTo(<span class="kwrd">string</span>.Empty));</pre>
                                                              
                                                              <pre><span class="lnum">  20:  </span>        }</pre>
                                                              
                                                              <pre class="alt"><span class="lnum">  21:  </span>    }</pre>
                                                              
                                                              <pre><span class="lnum">  22:  </span>}</pre>
                                                            </div>
                                                            
                                                            <p>
                                                              In this test I have created a list of Persons in one transaction and then I cleared the session in order to be sure that nothing was left in the cache. Then I loaded one of the Person entities using the Get<T> method and then I load it again using the same method call in order to verify that the SELECT statement was issued only once.
                                                            </p>
                                                            
                                                            <p>
                                                              <a href="http://raffaeu.com/wp-content/uploads/2013/03/d6927ff4-0600-48e4-a344-19ff630e002dimage_6.png"><img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/ddd309af-52e6-44de-b6f8-8670f5c2ad40image_thumb_2.png" width="531" height="248" /></a>
                                                            </p>
                                                            
                                                            <p>
                                                              As you can see, NHibernate is loading the entire entity from the database in the first call, and in the second one is simply loading it again from the level 1 cache. You should notice here that NHibernate is loading <strong>the entire entity</strong> in the first Get<T> call.
                                                            </p>
                                                            
                                                            <h3>
                                                              Load<T>()
                                                            </h3>
                                                            
                                                            <pre class="csharpcode">[Test]
[Category(<span class="str">"Database"</span>)]
<span class="kwrd">public</span> <span class="kwrd">void</span> UsingLoadThePersonIsPartiallyCached()
{
    <span class="kwrd">using</span> (var session = factory.OpenSession())
    {
        <span class="kwrd">using</span> (var tx = session.BeginTransaction(IsolationLevel.ReadCommitted))
        {
            var persons = MockFactory.MakePersons();
            persons.ForEach(p =&gt; session.Save(p));
            tx.Commit();
            session.Clear();
            Console.WriteLine(<span class="str">"*** FIRST SELECT ***"</span>);
            var expectedPerson1 = session.Load&lt;Person&gt;(persons[0].PersonId);
            Assert.That(expectedPerson1, Is.Not.Null);
            Console.WriteLine(<span class="str">"*** SECOND SELECT ***"</span>);
            var expectedPerson2 = session.Load&lt;Person&gt;(persons[0].PersonId);
            Assert.That(expectedPerson2, Is.Not.Null);
            Assert.That(expectedPerson2.FirstName, Is.Not.EqualTo(<span class="kwrd">string</span>.Empty));
        }
    }
}</pre>
                                                            
                                                            <p>
                                                              In this second test I am executing the same exact steps of the previous one, but this time I am using the Load<T> method and the result is completely different! Look at the SQL log below:
                                                            </p>
                                                            
                                                            <p>
                                                              <a href="http://raffaeu.com/wp-content/uploads/2013/03/cbda39a0-6f1a-4347-8e98-6c8eec217ec5image_8.png"><img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/f749fce2-2e94-4d25-ae2d-b28ba814da65image_thumb_3.png" width="536" height="242" /></a>
                                                            </p>
                                                            
                                                            <p>
                                                              Now NHibernate is not loading the entity from the database at all, it is loading it only in the second call, when I try to hit one of the Person properties. If you debug this code you will notice that NHibernate issues the database call at the line Assert.That(expectedPerson2.FirstName, Is.Not.EqualTo(<span class="kwrd">string</span>.Empty)); and not before!
                                                            </p>
                                                            
                                                            <h3>
                                                              Session maintenance
                                                            </h3>
                                                            
                                                            <p>
                                                              If you are working with the <em>ISession</em> object in a Client application or if you are keeping it alive in a web application using some strange behaviors like keeping it saved inside the <em>HttpContext</em> you will realize, soon or later, that sometimes the cache of level 1 needs to be cleared.
                                                            </p>
                                                            
                                                            <p>
                                                              Now, despite the fact that these methods (based on my personal experience) should never be used, because it means that you are wrongly implementing your data layer, and despite the fact that the behavior of these methods may result in something <em>unexpected</em>, NHibernate provides three different methods to <em>clear</em> the cache of level 1 content.
                                                            </p>
                                                            
                                                            <table border="0" cellspacing="0" cellpadding="2" width="400">
                                                              <tr>
                                                                <td valign="top" width="10">
                                                                   
                                                                </td>
                                                                
                                                                <td valign="top" width="117">
                                                                  <h4>
                                                                    Session.Clear
                                                                  </h4>
                                                                </td>
                                                                
                                                                <td valign="top" width="10">
                                                                   
                                                                </td>
                                                                
                                                                <td valign="top" width="108">
                                                                  <h4>
                                                                    Session.Evict
                                                                  </h4>
                                                                </td>
                                                                
                                                                <td valign="top" width="10">
                                                                   
                                                                </td>
                                                                
                                                                <td valign="top" width="169">
                                                                  <h4>
                                                                    Session.Flush
                                                                  </h4>
                                                                </td>
                                                                
                                                                <td valign="top" width="10">
                                                                   
                                                                </td>
                                                              </tr>
                                                              
                                                              <tr>
                                                                <td valign="top" width="10">
                                                                   
                                                                </td>
                                                                
                                                                <td valign="top" width="117">
                                                                   
                                                                </td>
                                                                
                                                                <td valign="top" width="10">
                                                                   
                                                                </td>
                                                                
                                                                <td valign="top" width="108">
                                                                   
                                                                </td>
                                                                
                                                                <td valign="top" width="10">
                                                                   
                                                                </td>
                                                                
                                                                <td valign="top" width="169">
                                                                   
                                                                </td>
                                                                
                                                                <td valign="top" width="10">
                                                                   
                                                                </td>
                                                              </tr>
                                                              
                                                              <tr>
                                                                <td valign="top" width="10">
                                                                   
                                                                </td>
                                                                
                                                                <td valign="top" width="117">
                                                                  <p align="left">
                                                                    Removed all the existing objects from the <em>ISession</em> <strong>without</strong> syncing them with the database
                                                                  </p>
                                                                </td>
                                                                
                                                                <td valign="top" width="10">
                                                                  <p align="left">
                                                                    </td> 
                                                                    
                                                                    <td valign="top" width="108">
                                                                      <p align="left">
                                                                        Remove a specific object from the <em>ISession</em> <strong>without</strong> syncing it with the database
                                                                      </p>
                                                                    </td>
                                                                    
                                                                    <td valign="top" width="10">
                                                                      <p align="left">
                                                                        </td> 
                                                                        
                                                                        <td valign="top" width="169">
                                                                          <p align="left">
                                                                            Remove all the existing objects from the session <strong>by syncing</strong> them with the database
                                                                          </p>
                                                                        </td>
                                                                        
                                                                        <td valign="top" width="10">
                                                                          <p align="left">
                                                                            </td> </tr> </tbody> </table> 
                                                                            
                                                                            <table border="0" cellspacing="0" cellpadding="2" width="400">
                                                                              <tr>
                                                                              </tr>
                                                                            </table>
                                                                            
                                                                            <p>
                                                                              I will probably write more about these three methods in some future post but if you need to investigate more about them, I would suggest you to read <strong>carefully</strong> the NHibernate docs available here: <a title="http://www.nhforge.org/doc/nh/en/index.html" href="http://www.nhforge.org/doc/nh/en/index.html">http://www.nhforge.org/doc/nh/en/index.html</a>
                                                                            </p>
                                                                            
                                                                            <p>
                                                                              In the next article we talk about the level 2 cache.
                                                                            </p>