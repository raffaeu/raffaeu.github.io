---
id: 334
title: 'SQL 2005 and Linked server (Cannot obtain the schema rowset &#8220;DBSCHEMA_TABLES_INFO&#8221;)'
date: 2008-06-19T01:00:27+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=334
permalink: /archive/2008/06/19/sql-2005-and-linked-server-cannot-obtain-the-schema-rowset-dbschema_tables_info.aspx
categories:
  - Microsoft Certifications
  - SQL Server
---
If you have a SQL 2005 machine with a (x64) installation, some of the system queries have a different name, like [some\_command]sys\_64. So when you try to connect your powerfull SQL2005 to an old remote SQL2000, probably in x32 version, you can receive a strange error like **Cannot obtain the schema rowset &#8220;DBSCHEMA\_TABLES\_INFO&#8221;**. 

In many forums you can find a link to a microsoft KB that explains to you that you have to install the SP4 in the old SQL and maybe everything will done.
  
Usually I like to know why something doesn&#8217;t run &#8230;

So, when you execute from SQL2005 a query like
  
_select * from sql2000.mybase.dbo.mytable
  
_ 
  
SQL Server 2005 x64 runs the following query on remote SQL2000 server:
  
_exec [mybase]..sp\_tables\_info\_rowset\_64 N&#8217;mytable&#8217;, N&#8217;dbo&#8217;, NULL_

So you can also try to add this stored in the **Remote SQL2000 server, in the master database**:

create procedure sp\_tables\_info\_rowset\_64
  
 <wbr>  <wbr>  @table_name sysname, 
  
 <wbr>  <wbr>  @table_schema <wbr>  <wbr>  sysname = null, <wbr>  
  
 <wbr>  <wbr>  @table_type nvarchar(255) = null 
  
as
  
declare @Result int set @Result = 0

exec @Result = sp\_tables\_info\_rowset @table\_name, @table\_schema, @table\_type

It works and you don&#8217;t need to run strange Package on your critical machine.

__