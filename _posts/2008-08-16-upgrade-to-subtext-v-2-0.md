---
id: 290
title: Upgrade to SubText V 2.0.
date: 2008-08-16T22:42:54+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=290
permalink: /archive/2008/08/16/upgrade-to-subtext-v-2-0.aspx
categories:
  - ASP.NET
  - 'C#'
  - NET World
---
This guide is made for who already has SubText installed and wants to upgrade to the latest release, the V 2.0.

### Pre-requisite.

First of all, starting from the version 1.9 subtext requires the ASP.NET 2.0 so be sure that your web server, or your hosting provider can give you the way to change the ASP.NET framework in IIS control panel.

You need a version of SQL Server. 

Your blog user that access the Database MUST have temporally DBO permission in order to apply some changes to the Database.

### First step, backup and upgrade new application.

First of all I suggest to you to backup your existing web site (SubText blog) and save the package locally in your computer or in a separate folder in your web site. In this way it should be very useful to rollback in case of failure.

Second step you should download from [SubText web site](http://www.subtextproject.com/) the [Version 2.0](http://sourceforge.net/project/showfiles.php?group_id=137896 "SubText V 2.0").

For a faster installation you should create into the wwwroot of your web server a new folder called subtext V 2.0 or something else and then redirect your domain to the new folder. 

This is my situation on [WH4L](http://www.webhost4life.com/) (Web Host for Life) web server:

<a href="http://blog.raffaeu.com/images/blog_raffaeu_com/WindowsLiveWriter/UpgradetoSubTextV2.0_13FC1/NewPicture.png" rel="lightbox"><img style="border-top-width: 0px; border-left-width: 0px; border-bottom-width: 0px; border-right-width: 0px" border="0" alt="New Picture" src="http://blog.raffaeu.com/images/blog_raffaeu_com/WindowsLiveWriter/UpgradetoSubTextV2.0_13FC1/NewPicture_thumb.png" width="167" height="76" /></a>

I have the wwwroot folder with two subtext package inside, the version 1.9.5 working and the new installation.

**If your installation is an upgrade, download the appropriate package and delete the database file contained into the App_Data folder of the new package.**

### Merge Information.

Before start I suggest to verify everything like:

  1. Check your Host Admin panel located at <http://yourblog.something/hostadmin> and be sure to access in, otherwise you can run this [query](http://subtextproject.com/Home/Docs/Upgrading/tabid/147/Default.aspx) to reset your host admin password. 
  2. Merge into the new web.config file the information from the old web.config of the previous configuration. Usually you should change information like: Database connection string, e-mail setting, Lightbox configuration and other third party configurations added previously to your blog. You can run a program like [WinMerge](http://winmerge.org/) very useful in this case.  
  3. Copy all the files you have customized into the new package. This include your image folder, it contains all the images of your old posts. Your video, example, personalized skin and other files you added after the installation of your previous package.

### Upgrade the blog.

Now it&#8217;s time to go to your host panel and point the domain to the new folder. After do that it should be very easy to rollback to the previous version in case of failure.

If everything it&#8217;s done well, you should see this page:

if you are not logged in as an Administrator this is the page your users will see until you will finish the Upgrade process.

<a href="http://blog.raffaeu.com/images/blog_raffaeu_com/WindowsLiveWriter/UpgradetoSubTextV2.0_13FC1/upgrade_screen.png" rel="lightbox"><img style="border-top-width: 0px; border-left-width: 0px; border-bottom-width: 0px; border-right-width: 0px" border="0" alt="upgrade_screen" src="http://blog.raffaeu.com/images/blog_raffaeu_com/WindowsLiveWriter/UpgradetoSubTextV2.0_13FC1/upgrade_screen_thumb.png" width="260" height="132" /></a> 

This is the page you will see if you are logged in as an administrator:

<a href="http://blog.raffaeu.com/images/blog_raffaeu_com/WindowsLiveWriter/UpgradetoSubTextV2.0_13FC1/upgrade.png" rel="lightbox"><img style="border-top-width: 0px; border-left-width: 0px; border-bottom-width: 0px; border-right-width: 0px" border="0" alt="upgrade" src="http://blog.raffaeu.com/images/blog_raffaeu_com/WindowsLiveWriter/UpgradetoSubTextV2.0_13FC1/upgrade_thumb.png" width="260" height="147" /></a> 

### Issues with Web Host for Life.

My web host for life doesn&#8217;t recognize this tag in the web.config of the new package

<EnclosureMimetypes>  
    <add key=&#8221;.mp3&#8243; value=&#8221;audio/mpeg&#8221;/>  
    <add key=&#8221;.mp4&#8243; value=&#8221;video/mp4&#8243;/>  
    <add key=&#8221;.zip&#8221; value=&#8221;application/octetstream&#8221;/>  
    <add key=&#8221;.pdf&#8221; value=&#8221;application/octetstream&#8221;/>  
    <add key=&#8221;.wmv&#8221; value=&#8221;video/wmv&#8221;/>  
    <add key=&#8221;.wma&#8221; value=&#8221;audio/wma&#8221;/>  
</EnclosureMimetypes> 

Actually I have commented this section, I will let you know what to do.

_Please Simone tell me what to do here. Thanks._