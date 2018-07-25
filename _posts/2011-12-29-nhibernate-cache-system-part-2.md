---
id: 68
title: NHibernate cache system. Part 2
date: 2011-12-29T20:23:27+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=68
permalink: /archive/2011/12/29/nhibernate-cache-system-part-2.aspx
categories:
  - 'C#'
  - NHibernate
---
In the <a href="http://blog.raffaeu.com/archive/2011/12/31/nhibernate-cache-system-part-1.aspx" target="_blank">previous post</a> we saw how the cache system is structured in NHibernate and how it works. We saw that we have different methods to play with the cache (Evict, Clear, Flush …) and they are all associated with the _ISession_ object because the cache of level 1 is associated with the lifecycle of an _ISession_ object.

In this second article we will see how the second level cache works and how it is associated with the _ISessionFactory_ object that is in charge of controlling this cache mechanism.

### Second Level cache architecture

How does the second level cache work?

[<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/5af1d94f-76f4-423e-be21-61645d96c6f2image_thumb.png" width="520" height="325" />](http://raffaeu.com/wp-content/uploads/2013/03/c7541729-c217-48c6-9521-2f847c12224cimage_2.png)

First of all, when an entity is cached in the second level cache, the entity is disassembled into a collection of keys/values pair, like a dictionary and persisted in the cache repository. This mechanism is accomplished because most of the second level cache providers are able to persist serialized dictionary collections and because in the same time NHibernate does not force you to make serializable your entities (something that IMHO, should never be done!!).

A second mechanism happens when we cache the result of a query (Linq, HQL, ICriteria) because these results can’t be cached using the first level cache (see previous blog post). After we cache a query result, NHibernate will cache **only** the unique identifiers of the entities involved in the result of the query.

Third, NHibernate has an internal mechanism that allows him to know and keep track of a timestamp value used to write tables or to work with sessions. How does it work? Well the mechanism is pretty clear, it keeps track of when the last table was written too. A series of mechanism will update this timestamp information and you can find a better explanation of Ayende’s blog: [http://ayende.com/blog/3112/nhibernate-and-the-second-level-cache-tips](http://ayende.com/blog/3112/nhibernate-and-the-second-level-cache-tips "http://ayende.com/blog/3112/nhibernate-and-the-second-level-cache-tips").

### Configuration of the second level cache

By default the second level cache is disabled. If you need to use the second level cache you have to let NHibernate know about that. The hibernate.cfg file has a dedicated section of parameters that should be used to enable the second level cache:

<div class="csharpcode">
  <pre class="alt"><span class="kwrd">&lt;</span><span class="html">property</span> <span class="attr">name</span><span class="kwrd">="cache.provider_class"</span><span class="kwrd">&gt;</span></pre>
  
  <pre>   NHibernate.Cache.HashtableCacheProvider</pre>
  
  <pre class="alt"><span class="kwrd">&lt;/</span><span class="html">property</span><span class="kwrd">&gt;</span></pre>
  
  <pre><font color="#9bbb59"><span class="kwrd">&lt;!</span><span class="html">--</span> <span class="attr">You</span> <span class="attr">have</span> <span class="attr">to</span> <span class="attr">explicitly</span> <span class="attr">enable</span> <span class="attr">the</span> <span class="attr">second</span> <span class="attr">level</span> <span class="attr">cache</span> <span class="attr">-</span><span class="kwrd">&gt;</span></font></pre>
  
  <pre class="alt"><span class="kwrd">&lt;</span><span class="html">property</span> <span class="attr">name</span><span class="kwrd">="cache.use_second_level_cache"</span><span class="kwrd">&gt;</span></pre>
  
  <pre>   true</pre>
  
  <pre class="alt"><span class="kwrd">&lt;/</span><span class="html">property</span><span class="kwrd">&gt;</span> </pre>
</div>

First of all we specify the cache provider we are using, in this case I am using the standard hashtable provider, but I will show you in the next article what are the real providers you should use. Second we say that the cache should be enabled; this part is really important because if you do not specify that the cache is enable, it simply won’t work … <img style="border-bottom-style: none; border-left-style: none; border-top-style: none; border-right-style: none" class="wlEmoticon wlEmoticon-confusedsmile" alt="Confused smile" src="http://raffaeu.com/wp-content/uploads/2013/03/65b8fdde-07ca-41a3-89ce-af1e8f4a143ewlEmoticon-confusedsmile_2.png" />

Then you may provide to the cache a default expiration in seconds:

<div class="csharpcode">
  <pre class="alt"><span class="rem">&lt;!-- cache will expire in 2 minutes --&gt;</span></pre>
  
  <pre><span class="kwrd">&lt;</span><span class="html">property</span> <span class="attr">name</span><span class="kwrd">="cache.default_expiration"</span><span class="kwrd">&gt;</span>120<span class="kwrd">&lt;/</span><span class="html">property</span><span class="kwrd">&gt;</span></pre>
</div>

<font color="#4d4d4d">If you want to add additional configuration properties, they will be cache provider specific!</font>

### Cache by mapping

One of the possible configuration is to enable the cache at the entity level. This means that we are marking our entity as _“cachable”_.

<div class="csharpcode">
  <pre class="alt"><span class="kwrd">&lt;</span><span class="html">class</span></pre>
  
  <pre>   <span class="attr">name</span><span class="kwrd">="CachableProduct“</span></pre>
  
  <pre class="alt">   table="[<span class="attr">CachableProduct</span>]“</pre>
  
  <pre>   <span class="attr">dynamic-insert</span><span class="kwrd">="true“</span></pre>
  
  <pre class="alt">   dynamic-update="<span class="attr">true</span>"<span class="kwrd">&gt;</span></pre>
  
  <pre>   <span class="kwrd">&lt;</span><span class="html">cache</span> <span class="attr">usage</span><span class="kwrd">="read-write"</span><span class="kwrd">/&gt;</span></pre>
</div>

In order to do that we have to introduce a new tag, the **<cache>** tag. In this tag we can specify different type of &#8220;_”usage”:_

  * **Read-write** 
    It should be used if you plan also to update the data (no with serializable transaction) </li> 
    
      * **Read-only** 
        Simplest and best performing, for read only access </li> 
        
          * **Nonstrict-read-write** 
            If you need to occasionally update the data. You must commit the transaction </li> 
            
              * **Transactional** 
                not documented/implemented yet because no one cache provider allows transactional cache. It is implemented in the Java version because J2EE allow transactional second level cache </li> </ul> 
                
                Now, if we write a simple test that will create some entities and will try to retrieve them using two different _ISession_ generated by the same _ISessionFactory_ we will get the following behavior:
                
                <div class="csharpcode">
                  <pre class="alt"><span class="kwrd">using</span> (var session = factory.OpenSession())</pre>
                  
                  <pre>{</pre>
                  
                  <pre class="alt">   <span class="rem">// create the products</span></pre>
                  
                  <pre>    <span class="kwrd">using</span> (var tx = session.BeginTransaction(IsolationLevel.ReadCommitted))</pre>
                  
                  <pre class="alt">    {</pre>
                  
                  <pre>        Console.WriteLine(<span class="str">"*** FIRST SESSION ***"</span>);</pre>
                  
                  <pre class="alt">        var expectedProduct1 = session.Get&lt;CachableProduct&gt;(productId);</pre>
                  
                  <pre>        Assert.That(expectedProduct1, Is.Not.Null);</pre>
                  
                  <pre class="alt">        tx.Commit();</pre>
                  
                  <pre>        <span class="rem">// retrieve the number of hits we did to the 2nd level cache</span></pre>
                  
                  <pre class="alt">        Console.WriteLine(<span class="str">"Second level hits {0}"</span>, <br />           factory.Statistics.SecondLevelCacheHitCount);</pre>
                  
                  <pre>    }</pre>
                  
                  <pre class="alt">}</pre>
                  
                  <pre><span class="kwrd">using</span> (var session = factory.OpenSession())</pre>
                  
                  <pre class="alt">{</pre>
                  
                  <pre>    <span class="kwrd">using</span> (var tx = session.BeginTransaction(IsolationLevel.ReadCommitted))</pre>
                  
                  <pre class="alt">    {</pre>
                  
                  <pre>        Console.WriteLine(<span class="str">"*** SECOND SESSION ***"</span>);</pre>
                  
                  <pre class="alt">        var expectedProduct1 = session.Get&lt;CachableProduct&gt;(productId);</pre>
                  
                  <pre>        Assert.That(expectedProduct1, Is.Not.Null);</pre>
                  
                  <pre class="alt">        tx.Commit();</pre>
                  
                  <pre>        <span class="rem">// retrieve the number of hits we did to the 2nd level cache</span></pre>
                  
                  <pre class="alt">        Console.WriteLine(<span class="str">"Second level hits {0}"</span>, <br />           factory.Statistics.SecondLevelCacheHitCount);</pre>
                  
                  <pre>    }</pre>
                  
                  <pre class="alt">}</pre>
                </div>
                
                The result will be the following:
                
                [<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/e725d3f4-a5af-42ff-882d-0f46dd98bebeimage_thumb_1.png" width="544" height="256" />](http://raffaeu.com/wp-content/uploads/2013/03/e9f2aea8-57a1-4d43-9eb2-19e6f576b6c3image_4.png)
                
                As you can see the second session will access the 2nd level cache using a transaction and will not use the database at all. This has been accomplished just by mapping the entity with the **<cache>** tag and by using the GET<T> method.
                
                Let’s make everything a little bit more complex. Let’s assume for a second that our object is an aggregate root and it is more complex than the previous one. If we want to cache also a collection of child or a parent reference we will need to change our mapping in the following way:
                
                <div class="csharpcode">
                  <pre class="alt"><span class="rem">&lt;!-- inside the product mapping file --&gt;</span></pre>
                  
                  <pre><span class="kwrd">&lt;</span><span class="html">bag</span> <span class="attr">name</span><span class="kwrd">="Attributes"</span> <span class="attr">cascade</span><span class="kwrd">="all"</span> <span class="attr">inverse</span> <span class="kwrd">="true"</span><span class="kwrd">&gt;</span></pre>
                  
                  <pre class="alt">  <span class="kwrd">&lt;</span><span class="html">cache</span> <span class="attr">usage</span><span class="kwrd">="read-write"</span><span class="kwrd">/&gt;</span></pre>
                  
                  <pre>  <span class="kwrd">&lt;</span><span class="html">key</span> <span class="attr">column</span><span class="kwrd">="ProductId"</span> <span class="kwrd">/&gt;</span></pre>
                  
                  <pre class="alt">  <span class="kwrd">&lt;</span><span class="html">one-to-many</span> <span class="attr">class</span><span class="kwrd">="CacheAttribute"</span><span class="kwrd">/&gt;</span></pre>
                  
                  <pre><span class="kwrd">&lt;/</span><span class="html">bag</span><span class="kwrd">&gt;</span> </pre>
                  
                  <pre class="alt"><span class="rem">&lt;!-- inside theCacheAttribute file --&gt;</span></pre>
                  
                  <pre><span class="kwrd">&lt;</span><span class="html">class</span></pre>
                  
                  <pre class="alt">    <span class="attr">name</span><span class="kwrd">="CacheAttribute"</span></pre>
                  
                  <pre>    <span class="attr">table</span><span class="kwrd">="[CacheAttribute]"</span></pre>
                  
                  <pre class="alt">    <span class="attr">dynamic-insert</span><span class="kwrd">="true"</span></pre>
                  
                  <pre>    <span class="attr">dynamic-update</span><span class="kwrd">="true"</span><span class="kwrd">&gt;</span></pre>
                  
                  <pre class="alt">  <span class="kwrd">&lt;</span><span class="html">cache</span> <span class="attr">usage</span><span class="kwrd">="read-write"</span><span class="kwrd">/&gt;</span></pre>
                  
                  <pre>  <span class="rem">&lt;!-- omit --&gt;</span></pre>
                  
                  <pre class="alt">  <span class="kwrd">&lt;</span><span class="html">many-to-one</span> </pre>
                  
                  <pre>     <span class="attr">class</span><span class="kwrd">="CachableProduct"</span> </pre>
                  
                  <pre class="alt">     <span class="attr">name</span><span class="kwrd">="Product"</span> <span class="attr">cascade</span><span class="kwrd">="all"</span><span class="kwrd">&gt;</span>   </pre>
                  
                  <pre>    <span class="kwrd">&lt;</span><span class="html">column</span> <span class="attr">name</span><span class="kwrd">="ProductId"</span> <span class="kwrd">/&gt;</span></pre>
                  
                  <pre class="alt">  <span class="kwrd">&lt;/</span><span class="html">many-to-one</span><span class="kwrd">&gt;</span></pre>
                  
                  <pre><span class="kwrd">&lt;/</span><span class="html">class</span><span class="kwrd">&gt;</span></pre>
                </div>
                
                Now we can execute the following test (I am omitting some parts for saving space, I hope you don’t mind …)
                
                <div class="csharpcode">
                  <pre class="alt"><span class="kwrd">using</span> (var session = factory.OpenSession())</pre>
                  
                  <pre>{</pre>
                  
                  <pre class="alt">    <span class="kwrd">using</span> (var tx = session.BeginTransaction(IsolationLevel.ReadCommitted))</pre>
                  
                  <pre>    {</pre>
                  
                  <pre class="alt">        Console.WriteLine(<span class="str">"*** FIRST SESSION ***"</span>);</pre>
                  
                  <pre>        var expectedProduct1 = session.Get&lt;CachableProduct&gt;(productId);</pre>
                  
                  <pre class="alt">        Assert.That(expectedProduct1, Is.Not.Null);</pre>
                  
                  <pre>        Assert.That(expectedProduct1.Attributes, Has.Count.GreaterThan(0));</pre>
                  
                  <pre class="alt">        tx.Commit();</pre>
                  
                  <pre>        Console.WriteLine(<span class="str">"Second level hits {0}"</span>, </pre>
                  
                  <pre class="alt">           factory.Statistics.SecondLevelCacheHitCount);</pre>
                  
                  <pre>    }</pre>
                  
                  <pre class="alt"> </pre>
                  
                  <pre>}</pre>
                  
                  <pre class="alt"><span class="kwrd">using</span> (var session = factory.OpenSession())</pre>
                  
                  <pre>{</pre>
                  
                  <pre class="alt">    <span class="kwrd">using</span> (var tx = session.BeginTransaction(IsolationLevel.ReadCommitted))</pre>
                  
                  <pre>    {</pre>
                  
                  <pre class="alt">        Console.WriteLine(<span class="str">"*** SECOND SESSION ***"</span>);</pre>
                  
                  <pre>        var expectedProduct1 = session.Get&lt;CachableProduct&gt;(productId);</pre>
                  
                  <pre class="alt">        Assert.That(expectedProduct1, Is.Not.Null);</pre>
                  
                  <pre>        Assert.That(expectedProduct1.Attributes, Has.Count.GreaterThan(0));</pre>
                  
                  <pre class="alt">        tx.Commit();</pre>
                  
                  <pre>        Console.WriteLine(<span class="str">"Second level hits {0}"</span>, </pre>
                  
                  <pre class="alt">           factory.Statistics.SecondLevelCacheHitCount);</pre>
                  
                  <pre>    }</pre>
                  
                  <pre class="alt">}</pre>
                </div>
                
                And this is the result from the profiled SQL:
                
                [<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/31644fe9-2066-4e15-b6b2-bf7baf31aab7image_thumb_2.png" width="536" height="449" />](http://raffaeu.com/wp-content/uploads/2013/03/d47c4d1b-ccdd-4361-8d22-5bb6c27fd48bimage_6.png)
                
                In this case the second _ISession_ is calling the cache 4 times in order to resolve all the objects (2 products x 2 categories).
                
                ### Cache a query result
                
                Another way to cache our result is by creating a _cachable query_ that is slightly different than creating a cachable object.
                
                <div style="border-bottom: #666666 1px solid; border-left: #666666 1px solid; padding-bottom: 3px; padding-left: 3px; padding-right: 3px; background: #efefef; border-top: #666666 1px solid; border-right: #666666 1px solid; padding-top: 3px">
                  <strong>Important note: </strong></p> 
                  
                  <p>
                    <em>In order to cache a query we need to set the query as “cachable” and then set the corresponding entity as “cachable” too. Otherwise NHB will cache the ID of the entity but then it will always fetch the entity and cache only the query result.</em>
                  </p>
                </div>
                
                To write a cachable query we need to implement an _IQuery_ object in the following way:
                
                <div class="csharpcode">
                  <pre class="alt"><span class="kwrd">using</span> (var session = factory.OpenSession()) {</pre>
                  
                  <pre>     <span class="kwrd">using</span> (var tx = session.BeginTransaction(IsolationLevel.ReadCommitted))</pre>
                  
                  <pre class="alt">     {</pre>
                  
                  <pre>        var result = session</pre>
                  
                  <pre class="alt">             .CreateQuery(<span class="str">"from CachableProduct p where p.Name = :name"</span>)</pre>
                  
                  <pre>             .SetString(<span class="str">"name"</span>, <span class="str">"PC"</span>)</pre>
                  
                  <pre class="alt">             .SetCacheable(<span class="kwrd">true</span>)</pre>
                  
                  <pre>             .List&lt;CachableProduct&gt;();</pre>
                  
                  <pre class="alt">        tx.Commit();</pre>
                  
                  <pre>     }</pre>
                  
                  <pre class="alt"> }</pre>
                </div>
                
                Now, let’s try to write a unit test for this:
                
                <div class="csharpcode">
                  <pre class="alt"><span class="kwrd">using</span> (var session = factory.OpenSession())</pre>
                  
                  <pre>{</pre>
                  
                  <pre class="alt">    <span class="kwrd">using</span> (var tx = session.BeginTransaction(IsolationLevel.ReadCommitted))</pre>
                  
                  <pre>    {</pre>
                  
                  <pre class="alt">        Console.WriteLine(<span class="str">"*** FIRST SESSION ***"</span>);</pre>
                  
                  <pre>        var result = session</pre>
                  
                  <pre class="alt">            .CreateQuery(<span class="str">"from CachableProduct p where p.Name = :name"</span>)</pre>
                  
                  <pre>            .SetString(<span class="str">"name"</span>, <span class="str">"PC"</span>)</pre>
                  
                  <pre class="alt">            .SetCacheable(<span class="kwrd">true</span>)</pre>
                  
                  <pre>            .List&lt;CachableProduct&gt;();</pre>
                  
                  <pre class="alt">        Console.WriteLine(<span class="str">"Cached queries {0}"</span>, </pre>
                  
                  <pre>             factory.Statistics.QueryCacheHitCount);</pre>
                  
                  <pre class="alt">        Console.WriteLine(<span class="str">"Second level hits {0}"</span>, </pre>
                  
                  <pre>             factory.Statistics.SecondLevelCacheHitCount);</pre>
                  
                  <pre class="alt">        tx.Commit();</pre>
                  
                  <pre>    }</pre>
                  
                  <pre class="alt">}</pre>
                  
                  <pre><span class="kwrd">using</span> (var session = factory.OpenSession())</pre>
                  
                  <pre class="alt">{</pre>
                  
                  <pre>    <span class="kwrd">using</span> (var tx = session.BeginTransaction(IsolationLevel.ReadCommitted))</pre>
                  
                  <pre class="alt">    {</pre>
                  
                  <pre>        Console.WriteLine(<span class="str">"*** SECOND SESSION ***"</span>);</pre>
                  
                  <pre class="alt">        var result = session</pre>
                  
                  <pre>            .CreateQuery(<span class="str">"from CachableProduct p where p.Name = :name"</span>)</pre>
                  
                  <pre class="alt">            .SetString(<span class="str">"name"</span>, <span class="str">"PC"</span>)</pre>
                  
                  <pre>            .SetCacheable(<span class="kwrd">true</span>)</pre>
                  
                  <pre class="alt">            .List&lt;CachableProduct&gt;();</pre>
                  
                  <pre>        Console.WriteLine(<span class="str">"Cached queries {0}"</span>, </pre>
                  
                  <pre class="alt">             factory.Statistics.QueryCacheHitCount);</pre>
                  
                  <pre>        Console.WriteLine(<span class="str">"Second level hits {0}"</span>, </pre>
                  
                  <pre class="alt">             factory.Statistics.SecondLevelCacheHitCount);</pre>
                  
                  <pre>        tx.Commit();</pre>
                  
                  <pre class="alt">    }</pre>
                  
                  <pre>}</pre>
                </div>
                
                And this is the expected result:
                
                [<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="image" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/89bfa0cb-ad90-47a1-a2a3-e9da8ed726c8image_thumb_3.png" width="409" height="286" />](http://raffaeu.com/wp-content/uploads/2013/03/f4b21697-4e04-47bb-843c-57f02c801e6bimage_8.png)
                
                In this case the cache is telling us that the second session has **1 query result cached** and that we called it once.
                
                ### Final advice
                
                As you saw using the 1st and 2nd level cache is a pretty straightforward process but it requires time and understanding of NHibernate cache mechanism. Below are some final advice that you should keep in consideration when working with the 2nd level cache:
                
                  * 2nd Level Cache is never aware of external database changes!
                  * Default cache system is hashtable, you must use a different one
                  * Wrong implementation of the 2nd level cache may result in a non expected performance degrade (i.e. hashtable doc)
                  * First level cache is shared across same ISession, second level is shared across same ISessionFactory
                
                In the <a href="http://blog.raffaeu.com/archive/2011/12/29/nhibernate-cache-system-part-3.aspx" target="_blank">next article</a> we will see what are the available cache providers.