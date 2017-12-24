---
id: 288
title: How to write IT Procedure for Audit.
date: 2008-08-30T22:10:05+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=288
permalink: /archive/2008/08/30/how-to-write-it-procedure-for-audit.aspx
categories:
  - Bermuda
  - Technical Documents
---
This is an unusual post for me, today I will not talk about C#, or about Design Patterns or about SQL but about IT audit.

In the past I have worked in a couple of banks in Switzerland and as a consultant. Now I&#8217;m working as an IT Manager in a finance company in Bermuda. One of the most non valued and non considered process in the IT departments are the procedures and technical manuals.   
But not for me.   
In my opinion one of the most important thing in a well maintained system is to keep live and up to date the procedures and the technical manuals. In this way in case of failure, in case of modify and in case of audit, the IT department will be ready to fly!!

Unfortunately Google doesn&#8217;t give us a lot of suggestions and examples, so I want to share with you my knowledge.

### What&#8217;s an IT procedure?

An IT procedure is a document, manual that explain a process or a system in a well knows format, readable also by non-IT people. The document must contains some critical information that I will classify today for you.

_Please don&#8217;t create IT procedure with 10 thousand different font and colors, because it&#8217;s an official technical document and non a graphic challenge!!_

### Main Structure.

Starting by the name, the document must represent what you are talking for. For example, if the document represent a backup procedure, the correct name syntax should be \[PIT\] \[number\] [procedure], where PIT means Procedure Information Technology, the number is a unique GUID assigned to the document, and the name in our example should be _Backup procedure_.

After the cover we should add the approval section. This section must be signed to give to the document a _real mean_.

[<img style="border-top-width: 0px; border-left-width: 0px; border-bottom-width: 0px; border-right-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/images/blog_raffaeu_com/WindowsLiveWriter/HowtowriteITProcedureforAudit_13D41/image_thumb.png" width="521" height="193" />](http://blog.raffaeu.com/images/blog_raffaeu_com/WindowsLiveWriter/HowtowriteITProcedureforAudit_13D41/image.png)

After the approval we have to add the **Table of Content** and to accomplish this step, if you&#8217;re using word, you can simply add a table of content object and use the Header 123456 to represent the various section of your manual.

### The various sections.

Every section of the document should represent a particular information. You don&#8217;t have to create a section for every step of the procedure, that part is the process section &#8230; Let&#8217;s see what I&#8217;m talking about.

#### Principal section [procedure name]. 

In this section you should explain the process you are going to document and add some extra sub section like: objective, description, scope, priority &#8230;

#### Authorization and roles. 

In this section you should explain who is involved in the process, which are the responsibilities and so on. Sub section are: authorization, roles (brief description), people involved in the manual.

#### Roles. 

This is the detailed section of the previous one. For every roles present in the procedure you have to create a subsection that explain in details the role, the scope of the role and the people who take part in the role.

#### Process. 

This section explains in details the process. I cannot make an example because for every process there different steps, but let&#8217;s try to imagine a backup procedure, the subsection could be: backup process, verify process and restore process.

Don&#8217;t forget to add a schematic flow that describe the process in all its content. Should be a diagram, a work flow or whatever you consider right.

#### Request forms. 

If the process contains one or more forms, here is the place where you have to add a small screen shot of these forms, for every form you should create a subsection that describe the form instead.

#### Key contacts. 

The key contacts is often the final section of an IT procedure. Here you have to add all the people involved in the document and in the process, why you should contact one of them, the exact role described previously in the document and so on &#8230; 

### External links.

I suggest some articles (my blog is not a bible for nobody) that can help you.

<a href="http://www.brainmass.com/homework-help/business/auditing/118599" target="_blank">How to write an audit approach</a>

<a href="http://www.riversideca.gov/audit/pdf/How%20to%20Write%20Procedures%20to%20Increase%20Control.pdf" target="_blank">How to write procedure to increase control</a>

<a href="http://www.metabolomics.ca/News/sops/HowToWriteAStandardOperatingProcedurev1.pdf" target="_blank">How to write a standard operating procedure (IT specific)</a>

<a href="http://www.companymanuals.com/" target="_blank">Compliance books (ASAP my personal one)</a>

Hope this will be helpful for someone. And enjoy your audit!!