---
id: 331
title: SQL SSIS, insert only new rows.
date: 2008-06-20T13:14:09+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=331
permalink: /archive/2008/06/20/sql-ssis-insert-only-new-rows.aspx
categories:
  - SQL Server
---
SSIS workflow is really amazing when you have to add some automatism in your data flow processes. But sometimes is better to make same checks before start with a really dangerous BULK operation.

One of the most noise stuff I have to do is to launch insert of new records when I receive updates from web (web services, XML data). But when I do this I&#8217;m always afraid about duplicated rows.

#### Ho to check if a row exist, in SSIS?

SSIS Package contains a component called lookup. Very intuitive name. So if you need to insert only some new rows in a table you can build a workflow like this:

<a href="http://raffaeu.com/wp-content/uploads/2013/03/8ea3ec7b-8f89-4459-b2c7-2987939a12b8image_2.png" rel="lightbox"><img style="border-top-width: 0px; border-left-width: 0px; border-bottom-width: 0px; border-right-width: 0px" height="215" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/4a7056a3-b26e-43a1-8dec-e837e647a9c2image_thumb.png" width="260" border="0" /></a> 

So we have three components:

  1. A OLEDB Datasource that reads data from the source table
  2. A LookUp component that check every row, if exists in the destination table. If doesn&#8217;t it redirects the rows in the **error event**.
  3. A Destination SQL that receive only the new rows, the rows that are in **error** in the lookup component.

Easy and automatic.