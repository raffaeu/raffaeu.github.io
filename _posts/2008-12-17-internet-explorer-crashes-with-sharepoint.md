---
id: 280
title: Internet Explorer crashes with Sharepoint.
date: 2008-12-17T10:48:12+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=280
permalink: /archive/2008/12/17/internet-explorer-crashes-with-sharepoint.aspx
categories:
  - ASP.NET
  - NET World
  - SharePoint
  - Technical Documents
---
This is a known issue that I discovered this morning.

#### Symptoms

In my company we are using Windows Sharepoint Services 3.0 with Windows 2008.  
The clients have windows XP SP3 with Office 2003 SP3.  
The browser is Internet Explorer 7.0.

Some clients have also Office 2007 or some part of it or some **compatibility pack**.

When they: open a document, edit a document, check-in, check-out, **Internet Explorer crashes**. 

#### Resolution

To solve the problem, Microsoft has realized a KB which must be downloaded here:

<http://support.microsoft.com/kb/938888>

This solved my issue in the network.