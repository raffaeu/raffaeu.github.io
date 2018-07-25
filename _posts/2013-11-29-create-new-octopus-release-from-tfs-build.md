---
id: 591
title: Create new Octopus Release from TFS Build
date: 2013-11-29T09:58:00+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=591
permalink: /archive/:year/:month/:day/:title/
categories:
  - Agile
  - TFS
---
In this article we will have a look at how we can automate the Octopus deployment using TFS build server. Every time a member of the team performs a check-in I want to execute a continuous build with the following workflow:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/11/image_thumb.png" width="420" height="178" />](http://blog.raffaeu.com/wp-content/uploads/2013/11/image.png)

The first step is to change the default build workflow in TFS. Usually I clone the default build workflow and work with a new one, cause if something goes wrong I can easily rollback to the default build status.

First of all we need to create a new version of our build workflow, so I clone my CI build and its own workflow:

**#01 – Clone the CI build**   
[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/11/image_thumb1.png" width="260" height="176" />](http://blog.raffaeu.com/wp-content/uploads/2013/11/image1.png)

**#02 – Clone the Workflow   
** 

In order to clone the workflow you just have to press the NEW button and locate the original workflow, or DOWNLOAD an existing one into your workspace:   
[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/11/image_thumb2.png" width="420" height="275" />](http://blog.raffaeu.com/wp-content/uploads/2013/11/image2.png)

Now, you need to locate a specific section of the workflow. We want to create a new release of our app only if everything went fine in the build but before the Gated Check-In is issued, because if we can’t publish to Octopus, the build still has to fail.

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/11/image_thumb3.png" width="257" height="260" />](http://blog.raffaeu.com/wp-content/uploads/2013/11/image3.png)

In my case I want to obtain the following output on my build in case of success or failure, plus I don’t want to publish a release if something went wrong in the build:

**#01 – Build log   
[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/11/image_thumb4.png" width="260" height="119" />](http://blog.raffaeu.com/wp-content/uploads/2013/11/image4.png)**

**#02 – Build summary   
[<img title="SNAGHTML13155776" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="SNAGHTML13155776" src="http://blog.raffaeu.com/wp-content/uploads/2013/11/SNAGHTML13155776_thumb.png" width="260" height="141" />](http://blog.raffaeu.com/wp-content/uploads/2013/11/SNAGHTML13155776.png)**

I also want to output a basic log so that I can debug my build just by reading the log.

Now the fun part, I need to execute the Octo.exe command from TFS in order to be able to publish my projects. I need to know few info that I will provide to my build workflow as **output parameters**:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/11/image_thumb5.png" width="370" height="108" />](http://blog.raffaeu.com/wp-content/uploads/2013/11/image5.png)

Finally, I have to create a new task in my workflow that will execute the command. How? 

[<img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/11/image_thumb6.png" width="370" height="409" />](http://blog.raffaeu.com/wp-content/uploads/2013/11/image6.png)

The trick is inside the **InvokeProcess** activity. In this activity I simply call Octo.exe and use the Octopus API to publish my project into the **Staging** environment. This is the environment where I will run my Automated Tests.

I configured the activity in the following way:

[<img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2013/11/image_thumb7.png" width="370" height="147" />](http://blog.raffaeu.com/wp-content/uploads/2013/11/image7.png)

You can find more information on how to call the Octopus API using Octo.exe here:   
<https://github.com/OctopusDeploy/Octopus-Tools/blob/master/readme.md>

Hope this help