---
id: 286
title: SQL Server, how to implement FULL TEXT search.
date: 2008-09-15T12:45:34+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=286
permalink: /archive/2008/09/15/sql-server-how-to-implement-full-text-search.aspx
categories:
  - SQL Server
---
One of the most useful function that I like in SQL Server is the FULL TEXT search. When you have a table or some tables with string fields, like VARCHAR or NVARCHAR, you know that running LIKE clause is CPU consuming. 

In order to run this example you must before activate the FULL TEXT search capability in your SQL engine. To do that:

  1. Open the **SQL Server surface area configuration.** 
  2. Open the **Services and Connections page**. 
  3. Configure the service to be enable.

Now you must run this Stored Procedure in order to activate the service in the Management Studio:

<div>
  <div class="csharpcode">
    <pre class="alt"><span class="lnum">   1:</span> <span class="kwrd">EXEC</span> sp_fulltext_database <span class="str">'enable'</span></pre></p>
  </div>
</div>

  
Last step is to activate the service for the tables, fields you want to use.

  1. Right click on the table and choose: **Full text index>>define new &#8230;** 
  2. Define the columns that will take part in the index, the schedule and the file group (consider a separated file group for big data)

Now we can run our examples. We will use the two clause for implementing a FULL TEXT search, the CONTAINS and the FREETEXT.
    
  
Please refer to the previous post for the table schema (<a title="Correlated queries" href="http://blog.raffaeu.com/archive/2008/09/14/sql-server-write-correlated-subqueries.aspx" target="_blank" rel="tag">here</a>).

### Using the CONTAINS clause.

The CONTAINS function search the exact word matches and word prefix matches. Let&#8217;s go to search all the addresses with the word Street.

<div>
  <div class="csharpcode">
    <pre class="alt"><span class="lnum">   1:</span> <span class="kwrd">SELECT</span> </pre>
    
    <pre class="alteven"><span class="lnum">   2:</span>     E.FirstName, E.LastName, A.Street</pre>
    
    <pre class="alt"><span class="lnum">   3:</span> <span class="kwrd">FROM</span> </pre>
    
    <pre class="alteven"><span class="lnum">   4:</span>     Employee <span class="kwrd">AS</span> E <span class="kwrd">INNER</span> <span class="kwrd">JOIN</span> Address <span class="kwrd">AS</span> A</pre>
    
    <pre class="alt"><span class="lnum">   5:</span> <span class="kwrd">ON</span></pre>
    
    <pre class="alteven"><span class="lnum">   6:</span>     E.Employee_id = A.Employee_fk</pre>
    
    <pre class="alt"><span class="lnum">   7:</span> <span class="kwrd">WHERE</span> <span class="kwrd">CONTAINS</span>(A.Street, <span class="str">'Street'</span>)</pre></p>
  </div>
</div>

And this is our result:

<div>
  <div class="csharpcode">
    <pre class="alt"><span class="lnum">   1:</span> Carl    Mark            Main Street</pre>
    
    <pre class="alteven"><span class="lnum">   2:</span> Carl    Mark            Front Street</pre>
    
    <pre class="alt"><span class="lnum">   3:</span> Pier    Francoise       Red Street</pre>
    
    <pre class="alteven"><span class="lnum">   4:</span> Mary    Stevenson       White main street</pre>
    
    <pre class="alteven"> </pre></p>
  </div>
</div>

In this case with the CONTAINS clause we will get all the rows where there is a word **Street** but not the rows where the word **Street**  is inside another word like: **MainStreet**. To do that we need this syntax: 

<div>
  <div class="csharpcode">
    <pre class="alt"><span class="lnum">   1:</span> <span class="kwrd">WHERE</span> <span class="kwrd">CONTAINS</span>(A.Street, <span class="str">'"*street"'</span>)</pre>
    
    <p>
      </div> </div> 
      
      <p>
        If you note, you need a double quote and a * before to find everything with <strong>street</strong> at the end.
      </p>
      
      <h3>
        Using the FREETEXT clause to get more results.
      </h3>
      
      <p>
        If you need to match all the result with the street word inside and similar results, you need to use the <strong>FREETEXT</strong> clause.
      </p>
      
      <p>
        With the freetext clause, SQL generates various forms of the search term, breaking single words into parts.
      </p>
      
      <div>
        <div class="csharpcode">
          <pre class="alt"><span class="lnum">   1:</span> <span class="kwrd">SELECT</span> </pre>
          
          <pre class="alteven"><span class="lnum">   2:</span>     E.FirstName, E.LastName, A.Street</pre>
          
          <pre class="alt"><span class="lnum">   3:</span> <span class="kwrd">FROM</span> </pre>
          
          <pre class="alteven"><span class="lnum">   4:</span>     Employee <span class="kwrd">AS</span> E <span class="kwrd">INNER</span> <span class="kwrd">JOIN</span> Address <span class="kwrd">AS</span> A</pre>
          
          <pre class="alt"><span class="lnum">   5:</span> <span class="kwrd">ON</span></pre>
          
          <pre class="alteven"><span class="lnum">   6:</span>     E.Employee_id = A.Employee_fk</pre>
          
          <pre class="alt"><span class="lnum">   7:</span> <span class="kwrd">WHERE</span> <span class="kwrd">FREETEXT</span>(A.Street, <span class="str">'street'</span>)</pre></p>
        </div>
      </div>
      
      <p>
        <br />The result here will be:
      </p>
      
      <div>
        <div class="csharpcode">
          <pre class="alt"><span class="lnum">   1:</span> Carl    Mark         Main Street</pre>
          
          <pre class="alteven"><span class="lnum">   2:</span> Carl    Mark         Front Street</pre>
          
          <pre class="alt"><span class="lnum">   3:</span> Pier    Francoise    Red Street</pre>
          
          <pre class="alteven"><span class="lnum">   4:</span> Pier    Francoise    mainstreet</pre></p>
        </div>
      </div>