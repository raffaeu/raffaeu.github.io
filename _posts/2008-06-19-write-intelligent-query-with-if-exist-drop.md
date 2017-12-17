---
id: 333
title: 'Write intelligent query with if exist &#8230; drop.'
date: 2008-06-19T17:34:30+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=333
permalink: /archive/2008/06/19/write-intelligent-query-with-if-exist-drop.aspx
categories:
  - Microsoft Certifications
  - SQL Server
---
Usually when we build a query we start the .sql file, or command in the query editor with a simple command &#8230; CREATE PROCEDURE &#8230; bla bla bla.

Ok but if I&#8217;m building a database from scratch is very probably that my procedure will be modified a lot of time. So every time we have to launch the command DROP PROCEDURE, or ALTER PROCEDURE.

This sounds good if you are building your object. But in case of you want to store as a _BACKUP PROCEDURE_ some scripts in the network, is better to produce this scripts as _ATOMIC and AUTOMATIC_.

To accomplish this task, we can use a function really useful in SQL SERVER, _IF [OBJECT] EXIST, DROP IT_!</p> 

<div class="wlWriterSmartContent" id="scid:57F11A72-B0E5-49c7-9094-E3A15BD5B5E7:3dbb90c1-2367-4d5d-8dee-bc010438711d" style="padding-right: 0px; display: inline; padding-left: 0px; float: none; padding-bottom: 0px; margin: 0px; width: 470px; padding-top: 0px">
  <pre style="background-color:White;white-space:-moz-pre-wrap; white-space: -pre-wrap; white-space: -o-pre-wrap; white-space: pre-wrap; word-wrap: break-word;;overflow: hidden;"><div>
  <!--

Code highlighting produced by Actipro CodeHighlighter (freeware)
http://www.CodeHighlighter.com/

-->
  
  <span style="color: #0000FF;">IF</span><span style="color: #000000;"> </span><span style="color: #808080;">EXISTS</span><span style="color: #000000;"> (</span><span style="color: #0000FF;">SELECT</span><span style="color: #000000;"> </span><span style="color: #808080;">*</span><span style="color: #000000;"> </span><span style="color: #0000FF;">FROM</span><span style="color: #000000;"> </span><span style="color: #FF0000;">[</span><span style="color: #FF0000;">dbo</span><span style="color: #FF0000;">]</span><span style="color: #000000;">.</span><span style="color: #FF0000;">[</span><span style="color: #FF0000;">sysobjects</span><span style="color: #FF0000;">]</span><span style="color: #000000;">
         </span><span style="color: #0000FF;">WHERE</span><span style="color: #000000;"> ID </span><span style="color: #808080;">=</span><span style="color: #000000;"> </span><span style="color: #FF00FF;">object_id</span><span style="color: #000000;">(N</span><span style="color: #FF0000;">'</span><span style="color: #FF0000;">[schema].[procedure]</span><span style="color: #FF0000;">'</span><span style="color: #000000;">) </span><span style="color: #808080;">AND</span><span style="color: #000000;"> 
               </span><span style="color: #FF00FF;">OBJECTPROPERTY</span><span style="color: #000000;">(id, N</span><span style="color: #FF0000;">'</span><span style="color: #FF0000;">IsProcedure</span><span style="color: #FF0000;">'</span><span style="color: #000000;">) </span><span style="color: #808080;">=</span><span style="color: #000000;"> </span><span style="color: #800000; font-weight: bold;">1</span><span style="color: #000000;">)
  </span><span style="color: #0000FF;">DROP</span><span style="color: #000000;"> </span><span style="color: #0000FF;">PROCEDURE</span><span style="color: #000000;"> </span><span style="color: #FF0000;">[</span><span style="color: #FF0000;">schema</span><span style="color: #FF0000;">]</span><span style="color: #000000;">.</span><span style="color: #FF0000;">[</span><span style="color: #FF0000;">procedure</span><span style="color: #FF0000;">]</span><span style="color: #000000;">
  </span><span style="color: #0000FF;">GO</span><span style="color: #000000;">
  </span><span style="color: #0000FF;">CREATE</span><span style="color: #000000;"> </span><span style="color: #0000FF;">PROCEDURE</span><span style="color: #000000;"> </span><span style="color: #FF0000;">[</span><span style="color: #FF0000;">schema</span><span style="color: #FF0000;">]</span><span style="color: #000000;">.</span><span style="color: #FF0000;">[</span><span style="color: #FF0000;">procedure</span><span style="color: #FF0000;">]</span><span style="color: #000000;"> 
  </span><span style="color: #0000FF;">AS</span><span style="color: #000000;">
  </span><span style="color: #0000FF;">BEGIN</span><span style="color: #000000;">
  </span>
</div></pre>
  
  <p>
    <!-- Code inserted with Steve Dunn's Windows Live Writer Code Formatter Plugin.  http://dunnhq.com --></div> 
    
    <p>
      So now you can run the script as much time as you want.
    </p>