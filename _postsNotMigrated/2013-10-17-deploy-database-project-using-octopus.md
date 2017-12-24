---
id: 567
title: Deploy Database Project using Octopus
date: 2013-10-17T18:38:00+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=567
permalink: /archive/:year/:month/:day/:title/
categories:
  - Agile
  - SQL Server
  - TFS
---
[Octopus](http://octopusdeploy.com/) is a deployment tool that use the <a href="http://www.nuget.org/" target="_blank">Nuget</a> packaging mechanism to pack your application and deploy it over multiple environments.

Unfortunately it does not have a native support (yet) for <a href="http://msdn.microsoft.com/en-us/library/dd193245.aspx" target="_blank">Visual Studio Database project</a>, so I had to provide a sort of workaround in my project structure to allow Octopus to deploy also a “Database Project <a href="http://www.nuget.org/" target="_blank">Nuget</a> package”.

### Visual Studio .dacpac

Visual Studio Database project is capable to generate diff scripts, a full schema deployment script and also a post deployment script (in case you need to populate the database with some demo data, for example). When you compile a Database project this is the outcome:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/10/image_thumb.png" width="247" height="147" />](http://blog.raffaeu.com/wp-content/uploads/2013/10/image.png)

As you can see we have two different **.dacpac** files. One for the master Database and one for my own database. A dacpac file is what is called [“Data Tier Application”](http://technet.microsoft.com/en-us/library/ee210546.aspx) and it’s used within SQL Server to deploy a database schema.

Another interesting thing is the **schema structure**, in every database project you will have also a second output folder with the following structure:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/10/image_thumb1.png" width="190" height="143" />](http://blog.raffaeu.com/wp-content/uploads/2013/10/image1.png)

And in the **obj** folder we have an additional output:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/10/image_thumb2.png" width="260" height="151" />](http://blog.raffaeu.com/wp-content/uploads/2013/10/image2.png)

which contains a **Model.xml** file. This file can be used to integrate entity framework with our database schema. The **postdeploy.sql** is a custom script that we generate and execute after the database deployment.

### Package everything with Nuget and OctoPack

So, what do we need in order to have a proper Nuget package of our database schema? Well, first of all let’s see what we should carry on in our package. Usually I create a package with the following structure:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/10/image_thumb3.png" width="370" height="163" />](http://blog.raffaeu.com/wp-content/uploads/2013/10/image3.png)

The steps to obtain this structure are the following:

1 – Modify the database project to run OctoPack 

<pre class="csharpcode"><span class="kwrd">&lt;</span><span class="html">Import</span> 
        <span class="attr">Project</span><span class="kwrd">="$(SolutionDir)\.nuget\NuGet.targets"</span> 
        <span class="attr">Condition</span><span class="kwrd">="Exists('$(SolutionDir)\.nuget\NuGet.targets')"</span> <span class="kwrd">/&gt;</span>
  <span class="kwrd">&lt;</span><span class="html">Import</span> 
        <span class="attr">Project</span><span class="kwrd">="$(SolutionDir)\.octopack\OctoPack.targets"</span> <span class="kwrd">/&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">Project</span><span class="kwrd">&gt;</span></pre>

2 – Provide a .nuspec file with the following structure:
    


<pre class="csharpcode"><span class="kwrd">&lt;?</span><span class="html">xml</span> <span class="attr">version</span><span class="kwrd">="1.0"</span>?<span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">package</span> <span class="attr">xmlns</span><span class="kwrd">="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd"</span><span class="kwrd">&gt;</span>
  <span class="kwrd">&lt;</span><span class="html">metadata</span><span class="kwrd">&gt;</span>
    <span class="rem">&lt;!-- Your file specifications --&gt;</span>
  <span class="kwrd">&lt;/</span><span class="html">metadata</span><span class="kwrd">&gt;</span>
  <span class="kwrd">&lt;</span><span class="html">files</span><span class="kwrd">&gt;</span>
    <span class="rem">&lt;!-- The Database Schema --&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">file</span> <span class="attr">src</span><span class="kwrd">="\dbo\**\*.sql"</span> 
            <span class="attr">target</span><span class="kwrd">="Content\Schema"</span><span class="kwrd">/&gt;</span>
    <span class="rem">&lt;!-- The deployment script --&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">file</span> <span class="attr">src</span><span class="kwrd">="\obj\**\*.sql"</span> 
            <span class="attr">target</span><span class="kwrd">="Content\Deploy"</span> <span class="kwrd">/&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">file</span> <span class="attr">src</span><span class="kwrd">="\obj\**\*.xml"</span> 
            <span class="attr">target</span><span class="kwrd">="Content\Deploy"</span> <span class="kwrd">/&gt;</span>
    <span class="rem">&lt;!-- Your .dacpac location --&gt;</span>
    <span class="kwrd">&lt;</span><span class="html">file</span> <span class="attr">src</span><span class="kwrd">="..\..\..\..\..\..\bin\**\*.dacpac"</span> 
            <span class="attr">target</span><span class="kwrd">="Content\Deploy"</span> <span class="kwrd">/&gt;</span>
  <span class="kwrd">&lt;/</span><span class="html">files</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">package</span><span class="kwrd">&gt;</span></pre>

And of course have your Build Server the **RunOctoPack** variable enabled.

### Install the package using Powershell

The final step to make the package “digestable” by Octopus using PowerShell. In our specific case we need a power shell script that can execute the .dacpac package and the post deployment script. That’s quite easy.

In order to install a .dacpac with power shell we can use this command:
    


<pre class="csharpcode"><span class="rem"># load Dac Pac</span>
add-type -path <span class="str">"C:\Program Files (x86)\Microsoft SQL Server\110\DAC\bin\Microsoft.SqlServer.Dac.dll"</span>

<span class="rem"># make DacServices object, needs a connection string </span>
$d = new-object Microsoft.SqlServer.Dac.DacServices <span class="str">"server=(local)"</span>

<span class="rem"># Load dacpac from file & deploy to database named pubsnew </span>
$dp = [Microsoft.SqlServer.Dac.DacPackage]::Load($DacPacFile) 
$d.Deploy($dp, $DatabaseName, $true)</pre>

In my case I set some variables in Octopus in order to be able to _dynamically_ create the database and locate the .dacpac file.

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/10/image_thumb4.png" width="505" height="163" />](http://blog.raffaeu.com/wp-content/uploads/2013/10/image4.png)

The final result is available through Octopus deployment console, cause I always set my PShell commands using **| Write-Host** at the end:

[<img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/10/image_thumb5.png" width="470" height="157" />](http://blog.raffaeu.com/wp-content/uploads/2013/10/image5.png)

**Final note**: remember that the only way to stop a deployment step in Octopus using Power Shell is to return –**1**. In my case I wrap the code in a Try/Catch and return –1 if you want to stop the deployment but you can find a <a href="http://octopusdeploy.com/blog/powershell-exit-codes" target="_blank">better explanation here</a>.