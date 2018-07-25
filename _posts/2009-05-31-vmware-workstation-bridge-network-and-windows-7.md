---
id: 266
title: VMWare Workstation, bridge network and Windows 7.
date: 2009-05-31T12:46:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=266
permalink: /archive/2009/05/31/vmware-workstation-bridge-network-and-windows-7.aspx
categories:
  - NET World
---
VMWare workstation is a very good product for basic virtualizations, like **having a personal netowork, implement a testing environment, test new products and/or operating systems**.

You can download an evaluation version of <a href="http://www.vmware.com/products/ws/" target="_blank">VMWare Workstation 6.5</a>, available also to support Windows 7 and x64 guest operating system.

#### Windows 7 and NET Framework 4.0

What I love about Microsoft products is the beta experience, that they offer always for free. At this link you can download an available virtual setup of Windows 7 with <a href="http://www.microsoft.com/downloads/details.aspx?FamilyId=922B4655-93D0-4476-BDA4-94CF5F8D4814&displaylang=en" target="_blank">Visual Studio 2010 and NET Framework 4</a>.

After that, in VMWare WRK you can simply import your Virtual PC machine and convert it into a VM disk.   
_If you prefer, you can download the ISO file of the Windows 7 and VS2010 64-bit in order to have a better experience into VMWare._

#### Bridge Networking

If you plan to add you Virtual Machine into your Host network, the only way is to use the Bridge Networking. 

> _Bridged networking means a virtual machine runs on a virtual network that is &#8220;connected&#8221; to an existing physical network. This permits a virtual machine to appear as a full-fledged host on an existing physical network._

First of all you need to open VMWare, and from the menu Edit, open the Virtual Network Editor.

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/VMWareWorkstationbridgenetworkandWindow_B3A5/image.png" rel="lightbox"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/VMWareWorkstationbridgenetworkandWindow_B3A5/image_thumb.png" width="260" height="213" /></a> 

Here you can go to **Host Virtual Network Mapping** and choose the **host adapter** that will became our bridged adapter. Letâ€™s say that we are using a wireless adapter with a subnet of 10.10.10.x, in this way our virtual machines will have the same subnet and will be available over the network.

After that, go into your Virtual Machine setup and change the network to **Bridged**.

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/VMWareWorkstationbridgenetworkandWindow_B3A5/image_3.png" rel="lightbox"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/VMWareWorkstationbridgenetworkandWindow_B3A5/image_thumb_3.png" width="260" height="204" /></a> 

#### Start the guest and change IP

Now we need to **tell** the guest system which IP it will need to have.   
For Windows 7, open Network panel and choose **Network and Sharing Center** and then **Change network adapter â€¦**

Open the TCP/IP and assign the IP you need, in order to make the machine visible to your network.

#### Share the Internet

Now, in order to give internet access also to our testing environment, letâ€™s go back to the Host network adapter (in my case the Vista Wireless adapter).

Choose properties and sharing:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/VMWareWorkstationbridgenetworkandWindow_B3A5/image_4.png" rel="lightbox"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/VMWareWorkstationbridgenetworkandWindow_B3A5/image_thumb_4.png" width="212" height="260" /></a> 

Now you can test this, by simply open the browser in the GUEST computer and type a web address.

#### Final step, RDP into the GUEST system

Now what we want to do is to work into the Guest, so we need to open an RDP connection and type the Guest IP address.

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/VMWareWorkstationbridgenetworkandWindow_B3A5/image_5.png" rel="lightbox"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/VMWareWorkstationbridgenetworkandWindow_B3A5/image_thumb_5.png" width="260" height="165" /></a> 

Do you want to use Aero from the HOST? Go into the properties of you remote connection and choose

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/VMWareWorkstationbridgenetworkandWindow_B3A5/image_6.png" rel="lightbox"><img style="border-bottom: 0px; border-left: 0px; display: inline; border-top: 0px; border-right: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/VMWareWorkstationbridgenetworkandWindow_B3A5/image_thumb_6.png" width="233" height="260" /></a>