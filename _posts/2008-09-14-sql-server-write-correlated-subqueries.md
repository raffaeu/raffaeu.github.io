---
id: 287
title: SQL server, write correlated subqueries.
date: 2008-09-14T12:04:05+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=287
permalink: /archive/2008/09/14/sql-server-write-correlated-subqueries.aspx
categories:
  - SQL Server
---
A **subquery** is a query that is nested into another query. When you work with T-SQL you have different ways to use subqueries, and every method has different performance results.

Our example is a Database with two tables, take a look on the image below:

<a href="http://raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/SQLserverwritecorrelatedsubqueries_A9BA/SubQueries.png" rel="lightbox"><img style="border-right: 0px; border-top: 0px; border-left: 0px; border-bottom: 0px" height="109" alt="SubQueries" src="http://raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/SQLserverwritecorrelatedsubqueries_A9BA/SubQueries_thumb.png" width="244" border="0" /></a> 

** **In this case we have a main table **Employee** with a one-to-many relation to the **Address** table.

### Where value IN. (non correlated example)

The first query I want to show you is:

<div>
  <div class="csharpcode">
    <pre class="alt"><span class="lnum">   1:</span> <span class="kwrd">SELECT</span> </pre>
    
    <pre class="alteven"><span class="lnum">   2:</span>     E.FirstName, E.LastName</pre>
    
    <pre class="alt"><span class="lnum">   3:</span> <span class="kwrd">FROM</span> </pre>
    
    <pre class="alteven"><span class="lnum">   4:</span>     Employee <span class="kwrd">AS</span> E</pre>
    
    <pre class="alt"><span class="lnum">   5:</span> <span class="kwrd">WHERE</span> </pre>
    
    <pre class="alteven"><span class="lnum">   6:</span>     E.Employee_id <span class="kwrd">IN</span></pre>
    
    <pre class="alt"><span class="lnum">   7:</span> (<span class="kwrd">SELECT</span> </pre>
    
    <pre class="alteven"><span class="lnum">   8:</span>     A.Employee_fk </pre>
    
    <pre class="alt"><span class="lnum">   9:</span> <span class="kwrd">FROM</span> Address <span class="kwrd">AS</span> A)</pre></p>
  </div>
</div>

  
In this case the SELECT will search all the Employee id in the address table and will return a list of id used by the first query to show the employee found. In this case we will not view the employees without and address. This is a non correlated example, because SQL will search before in the Address table, without know nothing about the employee table, than it will match the list of id with the employee table.

### Where value EXISTS. (correlated example)

The second query will use the EXISTS clause:

<div>
  <div class="csharpcode">
    <pre class="alt"><span class="lnum">   1:</span> <span class="kwrd">SELECT</span> </pre>
    
    <pre class="alteven"><span class="lnum">   2:</span>     E.FirstName, E.LastName</pre>
    
    <pre class="alt"><span class="lnum">   3:</span> <span class="kwrd">FROM</span> </pre>
    
    <pre class="alteven"><span class="lnum">   4:</span>     Employee <span class="kwrd">AS</span> E</pre>
    
    <pre class="alt"><span class="lnum">   5:</span> <span class="kwrd">WHERE</span> <span class="kwrd">EXISTS</span></pre>
    
    <pre class="alteven"><span class="lnum">   6:</span> (<span class="kwrd">SELECT</span> </pre>
    
    <pre class="alt"><span class="lnum">   7:</span>     A.Employee_fk </pre>
    
    <pre class="alteven"><span class="lnum">   8:</span> <span class="kwrd">FROM</span> </pre>
    
    <pre class="alt"><span class="lnum">   9:</span>     Address <span class="kwrd">AS</span> A</pre>
    
    <pre class="alteven"><span class="lnum">  10:</span> <span class="kwrd">WHERE</span> </pre>
    
    <pre class="alt"><span class="lnum">  11:</span>     E.Employee_id = A.EMployee_fk)</pre></p>
  </div>
</div>

  
In this case the query is using a correlated value from the outer query. The inner query match the id&#8217;s with the id&#8217;s in the outer query.

The performance for this queries are totally different if you are working with thousand of rows.

To understand which one is better, you should run both queries with an execution plan and try to build the right clustered index to run the most performing query.