---
id: 251
title: Real guide, configure Exchange 2003 SP2 and iPhone 3.0 OS.
date: 2009-08-08T16:20:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=251
permalink: /archive/2009/08/08/real-guide-configure-exchange-2003-sp2-and-iphone-3-0-os.aspx
categories:
  - ALT
  - Mobile
  - Technical Documents
---
Hi guys, as I told you in the previous post, I bought a new iPhone 3GS with the new OS 3.1. The phone is pretty cool, it works great and it has tons of software, utilities and services that I didnâ€™t find in any smart phone, including the BlackBerry.

The only big problem was to configure my business exchange email. Simply, it was not working until today. So in this small guide I am going to explain step by step what you have to do in order to have a full functional and working iPhone!

### Configure ActiveSync on Exchange Server 2003.

Before starting you must successful enable ActiveSync and OMA (Outlook mobile access) into your Exchange Server. If this step will not be successful, **do not go ahead!!**

In order to use ActiveSync on your exchange 2003 you must have:

  1. Windows Server (standard or enterprise) with the latest updates and the Service Pack 2 working.
  2. Exchange 2003 with latest updates and Service Pack 2 installed and working.
  3. Outlook Web Access working and accessible from outside your Network.
  4. Firewall ports (443, 80) opened for OWA.

If you cannot satisfy one of these prerequisites, please **Google** and search for a solution. I am not going to explain here how to enable a port into your firewall or how to install OWA in Exchange Server 2003, as in my opinion, out of topic.

After you have successfully installed **ActiveSync**, you will have this additional option inside your server, you have to right click on **global settings>mobile services** and choose _properties_ :

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/RealguideconfigureExchange2003SP2andiPh_E8D0/image.png" rel="lightbox[activesync]"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/RealguideconfigureExchange2003SP2andiPh_E8D0/image_thumb.png" width="260" height="182" /></a> 

You have to enable everything in order to have full access to the OMA service.   
After that, you should now download and install the <a href="http://www.microsoft.com/downloads/details.aspx?FamilyID=e6851d23-d145-4dbf-a2cc-e0b4c6301453&displaylang=en" target="_blank">Microsoft Exchange Server ActiveSync Web Administration Tool</a>, and handful tool that allows web administration of your smart phones. 

After you will install the tool, you will have this web application in Exchange local host:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/RealguideconfigureExchange2003SP2andiPh_E8D0/image_3.png" rel="lightbox[activesync]"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/RealguideconfigureExchange2003SP2andiPh_E8D0/image_thumb_3.png" width="260" height="170" /></a> 

The address is <https://localhost/mobileadmin> and for default is SSL enabled.

### Troubleshoot OMA installation.

This step is fundamental. If you cannot access the OMA web site, **you will not be able to use your iPhone or any other smart phone**.

Give it a try, inside Exchange, by typing the address <https://localhost/OMA> and insert your Exchange credentials. If you will see a text page with your mail folders, you are fine, otherwise you need to troubleshoot the problem â€¦ If itâ€™s working go directly to the iPhone Configuration step.

First of all open exchange active directory inside exchange and click **exchange tasks** over your user account. You have to enable all the futures, like this screenshot:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/RealguideconfigureExchange2003SP2andiPh_E8D0/image_4.png" rel="lightbox[activesync]"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/RealguideconfigureExchange2003SP2andiPh_E8D0/image_thumb_4.png" width="260" height="179" /></a> 

If itâ€™s not working yet â€¦ like in my situation, you have to configure a new virtual directory for OMA. In order to configure the virtual directory, follow, <a href="http://support.microsoft.com/kb/817379" target="_blank">step by step this Microsoft TechNet article</a> or <a href="http://support.microsoft.com/kb/330463" target="_blank">the HTTP error codes</a>. It solved my problem.

**_<u>Remember: until you get the OMA web site working, do not try to configure the iPhone!</u>_**

### Configure your iPhone.

I have an iPhone 3GS with OS 3.1, so probably if you have an older version, the screenshot below, will not be the same as mine.

Open the iPhone menu, click over **Settings** and then the menu **Mail, Contacts and Calendars**.

Insert a new account, choose an **Exchange account**.

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/RealguideconfigureExchange2003SP2andiPh_E8D0/image_5.png" rel="lightbox[activesync]"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/RealguideconfigureExchange2003SP2andiPh_E8D0/image_thumb_5.png" width="180" height="260" /></a> 

**Server:** I typed my web mail address, like webmail.mydomain.com, without https. Then I choose the SSL enabled communication. You if you have a **public URL for OMA** then you have to type that address.   
**<u>Do not type something like mymachinename.domain.whatever</u>** of course it will not work because your iPhone is not in your Office Network.

**Username:** itâ€™s my Windows username I use in the office.

**Domain:** itâ€™s my Active directory domain.

After that, you will be prompted for which objects you want to keep sync. I choose all of them like this screenshot:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/RealguideconfigureExchange2003SP2andiPh_E8D0/image_6.png" rel="lightbox[activesync]"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/RealguideconfigureExchange2003SP2andiPh_E8D0/image_thumb_6.png" width="180" height="260" /></a> 

After this step, after 5 minutes, you will be able to open your email and work with **push technology**, this means that you will be able to work directly inside your mail box from your iPhone. For example, if you add an event in the iPhone calendar, this will be added also into your mailbox in the office, and so on â€¦

This will be the final result, pretty cool!!

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/RealguideconfigureExchange2003SP2andiPh_E8D0/image_7.png" rel="lightbox[activesync]"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/RealguideconfigureExchange2003SP2andiPh_E8D0/image_thumb_7.png" width="213" height="260" /></a> 

Enjoy and please beware about the iPhones screenshots, for each version of the OS (2, 2.1, 3, 3.1) they change a little bit, but the information are always the same.

### Personal Considerations from BlackBerry to iPhone.

  * **<u>Advantages</u>**
  * IPhone is more useful, the usability is better than BlackBerry and the screen more clear.
  * The contacts search, merge and copy options are awesome. BB is not so awesome â€¦
  * The emails can be zoomed just with a finger, with the BB you cannot.
  * You can read Office, Acrobat, Images and Video in the mails, BB cannot.

  * **<u>Disadvantages</u>**
  * The most important is the battery life. My iPhone in a production environment doesnâ€™t live more then 10 hours. The BB has a battery life of 3/4 days, always on.

Finally, Microsoft Exchange is **the best** as an Email server and the **activesync** service is very good, but the best match for me is Exchange server with IPhone, very impressive!!

Additional Resources:

[http://support.apple.com](http://support.apple.com "http://support.apple.com")

[http://www.petri.co.il/configure_oma.htm](http://www.petri.co.il/configure_oma.htm "http://www.petri.co.il/configure_oma.htm")

[http://www.msexchange.org/tutorials/Managing-Mobile-Access-Exchange-Server-2003.html](http://www.msexchange.org/tutorials/Managing-Mobile-Access-Exchange-Server-2003.html "http://www.msexchange.org/tutorials/Managing-Mobile-Access-Exchange-Server-2003.html")

Tags: <a href="http://technorati.com/tag/iphone" rel="tag">iphone</a> <a href="http://technorati.com/tag/activesync" rel="tag">activesync</a> <a href="http://technorati.com/tag/exchange 2003" rel="tag">exchange 2003</a> <a href="http://technorati.com/tag/OMA" rel="tag">OMA</a>