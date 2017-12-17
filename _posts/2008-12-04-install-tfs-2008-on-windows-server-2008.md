---
id: 282
title: Install TFS 2008 on Windows Server 2008
date: 2008-12-04T19:09:24+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=282
permalink: /archive/2008/12/04/install-tfs-2008-on-windows-server-2008.aspx
categories:
  - Microsoft Certifications
  - NET World
  - SQL Server
---
Last week I decided to buy the TFS license for the TFS 2008. After 2 days and a lot of headaches I decided to follow step by step the official guide which is available here: [Team Foundation installation guide](http://www.microsoft.com/downloads/details.aspx?FamilyId=FF12844F-398C-4FE9-8B0D-9E84181D9923&displaylang=en) and to be honest, if you follow, step by step the guide, you will have a full working version of TFS. Also my MVP friend [Simon Chiaretta](http://codeclimber.net.nz) suggested me to use the official guide.

This short guide will show you how to install a **stand alone version**, this means, a single server version which will contains: WSS 3.0, SQL Standard 2008 and TFS 2008. So first of all we need 3 licenses, SQL Server 2008 standard (the express edition is not working with TFS), WSS 3.0 and Windows Server Standard 2008 and TFS 2008.

**Hey buddy, my suggestion is &#8220;**_spend more money and buy a x64 server with a SCSI disk system, otherwise everything will be very slow &#8230;_**&#8220;**

  1. **First of all, install windows server 2008  
** Probably this is the most straightforward step. 
      1. I don&#8217;t suggest any particular installation here, just a couple of few points. Install Windows Server 2008 standard, configure it as an **Application Server** and a **Web Server.** 
      2. Install IIS 7.0 and the NET Framework 3.5 with SP1 and verify it works, you can just point with the browser to <http://localhost> 
      3. Create a **dedicated account for the installation**, you must have administrative rights with the account, otherwise you will encounter a lot of trouble &#8230;
  2. **Install SQL Server 2008 Standard** 
  1. Lunch the setup for SQL Server 2008 Standard edition. 
  2. You must install the Server components, the Client components, the Reporting Services, Full-text search, Analysis services and the Management Studio if you will work locally into your machine. 
  3. For every service, specify at least, 1 account, otherwise after the installation you will not be able to login into SQL. I know it&#8217;s non sense but it&#8217;s how works &#8230; Also use only Windows Authentication. 
  4. Remember to reboot the machine after the installation and verify the server is running by connecting with the Management Studio. **Don&#8217;t configure the Reporting Services, this step will be done by TFS**. Yeah, I tried to configure it like integrated with MOSS and nothing were working properly. 
  5. If you receive during the installation process the error for the Power shell you did one of this wrong steps: you already have SQL 2005, you are using a CTP version of SQL which is coming with an old version of the power shell, you are using a wrong setup of SQL, you are using the Express with advanced services.

  3. **Install WSS 3.0 on Windows server 2008**
  1. If you are here this means that you have done your SQL installation (cool, it takes 3 times for me). Now download WSS 3.0 with SP1 from here: [WSS 3.0 SP1](http://www.microsoft.com/downloads/details.aspx?FamilyId=EF93E453-75F1-45DF-8C6F-4565E8549C2A&displaylang=en). Don&#8217;t download the WSS 3.0, it doesn&#8217;t work with Windows 2008, no way!! 
  2. Run the setup and choose the **advanced** installation. You have to choose the front-end installation and choose where you want to locate the installation directory. 
  3. After the installation is done, choose to **build a new server farm**, then choose the local server name, leave the database name as is, and with this syntax _domain\user_ provide the credential for the TFS administrator user. Check the box for the port, **don&#8217;t use the suggested port** and take a note of the port you will use, then choose the **NTLM** authentication. 
  4. Check your settings and **after you take a note of everything** proceed with the configuration of WSS. It will take a while! 
  5. **Some prompt for WSS 3.0**
  1. Now we need to play with the DOS. First open prompt and change directory to : Drive:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\bin\ 
  2. stsadm.exe -o extendvs -exclusivelyusentlm -url http://WSSServerName:80 -ownerlogin Domain\UserName -owneremail &#8220;admin@localhost&#8221; -sitetemplate sts -description &#8220;Default Web Site&#8221; 
  3. stsadm.exe -o siteowner -url http://WSSServerName:80 -secondarylogin Domain\TFSSETUP 
  4. The first will be the administrator of SharePoint, the second one the admin account for TFS. I use the same account &#8230;

  4. **Install step by step Team Foundation Server 2008**
  1. First of all, let&#8217;s install the TFS server. Choose the database server (local machine name) and go ahead. 
  2. Warning. I have received the warning that my SQL Server is not working, because I need [TFS SP1](http://msmvps.com/blogs/vstsblog/archive/2008/08/11/visual-studio-2008-net-2-5-and-tfs-2008-service-pack-1-released.aspx). Integrate the SP1 of TFS into the DVD is not easy, but you can find in the latest .chm the description. 
  3. Configure the account for TFS and SQL Server. 
  4. Choose the previous WSS address and site (did you take note of the port??)

The next post will be **how to configure TFS 2008**, when I will have time to do it.