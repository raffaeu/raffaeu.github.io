---
id: 254
title: T-SQL copy and clone an existing table.
date: 2009-07-20T13:20:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=254
permalink: /archive/2009/07/20/t-sql-copy-and-clone-an-existing-table.aspx
categories:
  - SQL Server
---
Something really useful that I like in Microsoft Access, is the capacity to select and **clone** an object just with a menu command.

The options we have in access are:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TSQLcopyandcloneanexistingtable_A727/image.png" rel="lightbox[COPYCLONE]"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TSQLcopyandcloneanexistingtable_A727/image_thumb.png" width="244" height="130" /></a> 

**Structure only** â€“ will copy/paste into a new table the table structure, so the index, the primary key and so on.

**Structure and Data** â€“ will copy/paste everything inside a new table.

**Append** â€“ will copy the data into the existing cloned object.

### What can we do with SQL Server?

Of course with SQL Server we cannot simply do that, especially if we are considering to automate this process inside a script or better, inside a job.

The first idea is to use the simple script SELECT INTO in this way:

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=IF&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">IF</a>  <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=EXISTS&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">EXISTS</a> 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:    (<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=SELECT&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">SELECT</a> * 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:    <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=FROM&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">FROM</a> sys.objects 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:    <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=WHERE&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">WHERE</a> object_id = OBJECT_ID(N'<span style="color: #8b0000">[schema].[tbl_pippo]</span>') 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:    <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=AND&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">AND</a> type <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">in</a> (N'<span style="color: #8b0000">U</span>'))
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6: <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=DROP&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">DROP</a> <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=TABLE&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">TABLE</a> [<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=schema&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">schema</a>].[tbl_pippo]
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7: <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=GO&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">GO</a>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9: <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=INSERT&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">SELECT</a> * 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10: <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=INTO&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">INTO</a> [<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=schema&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">schema</a>].[tbl_pippo]
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 11: <a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=FROM&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">FROM</a> [<a style="color: #0000ff" href="http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=schema&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99">schema</a>].[tbl_pluto]</pre>


<p>
  In this way we will copy: the records, the table structure but <strong>not the index or the primary keys</strong>.
</p>


<p>
  We can try to use the right click command <strong>Drop and Create</strong> and then change the destination name of our table:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TSQLcopyandcloneanexistingtable_A727/NewPicture9.png" rel="lightbox[COPYCLONE]"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="New Picture (9)" border="0" alt="New Picture (9)" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TSQLcopyandcloneanexistingtable_A727/NewPicture9_thumb.png" width="370" height="112" /></a> 
</p>


<p>
  Finally, we can use the SSIS service and create a powerful package able to do that. In this way we can be sure we are going to clone everything and decide if we want to copy/paste also the data.
</p>


<p>
  <strong>Transfer SQL Server object task. </strong>This task will do everything we need to copy and paste an object inside SQL:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TSQLcopyandcloneanexistingtable_A727/image_3.png" rel="lightbox[COPYCLONE]"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/TSQLcopyandcloneanexistingtable_A727/image_thumb_3.png" width="260" height="249" /></a> 
</p>


<p>
  We can also use some variables in order to make our task dynamic.
</p>


<p>
  Pretty cool, isnâ€™t it?
</p>


<p>
  Tags: <a href="http://technorati.com/tag/SSIS" rel="tag">SSIS</a> <a href="http://technorati.com/tag/copy SQL table" rel="tag">copy SQL table</a>
</p>