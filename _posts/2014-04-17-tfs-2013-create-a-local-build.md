---
id: 610
title: TFS 2013 Create a local build
date: 2014-04-17T20:00:00+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=610
permalink: /archive/:year/:month/:day/:title/
categories:
  - Agile
  - TFS
tags:
  - TFS
---
With TFS we can have two different type of Build, local or remote. A remote build in triggered on a controller that doesn’t reside on your local PC. A local build is triggered on your local dev agent and it can also be “hidden” from the main queue build repository. 

## The scenario

My scenario is the following:

_I have to commit a code change and I want to test the CI build locally before check-in my changes and commit the code to the main repository. I don’t want to work with Shelvesets cause I just don’t want to keep busy the main Build Controller_._&#160;_

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2014/04/image_thumb.png" width="260" height="181" />](http://blog.raffaeu.com/wp-content/uploads/2014/04/image.png)

So, for every build you queue (local or remote) the build agent will just create a new workspace and download the required files that need to be built.

So in my local PC I will end up with the following situation:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2014/04/image_thumb1.png" width="260" height="140" />](http://blog.raffaeu.com/wp-content/uploads/2014/04/image1.png)

Which is really inconvenient because it will just replicate my workspace for each build agent I am running locally and it won’t include the changes I didn’t commit to the repository.

So, first of all we want to instruct TFS to use a different strategy when running a local build than the strategy when running a remote build. 

Second we want to instruct the build agent to execute the build within the workspace directory without creating a new workspace and without downloading the latest files from the source because our local workspace **is the source**.

## How does TFS get the latest sources?

In order to understand my solution we need to have a look at how TFS build the workspace and what activities in the workflow are in charge of that. If you open the default build workflow (<a href="http://msdn.microsoft.com/en-us/library/dd647551.aspx" target="_blank">please refer here if you don’t know what I am talking about</a>) you will find that is starts with the following activities:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2014/04/image_thumb2.png" width="235" height="127" />](http://blog.raffaeu.com/wp-content/uploads/2014/04/image2.png)

**Initialize environment**

This activity setup the initial values for the Target folder, the bin folder and the test output folder. _You want to get rid of this activity because it will override your workspace_.

**Get sources**

This activity creates a new Workspace locally and download the latest code. You can pass a name for the workspace but unfortunately TFS will always drop the existing one and re-create it, so _this activity should also be removed from your local build definition_.

## Convert the remote to local path

At this point we need to inform TFS about the project location. Because we didn’t generate a workspace, when we ask TFS to build **$/MyProject/MyFile.cs** it will bomb saying that he doesn’t know how to translate a server path into a local path. Actually the real error is a bit misleading cause it just says “_I can’t find the file …_”

This error can be easily fixed by converting the projects to build into local path using the following TFS activities:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2014/04/image_thumb3.png" width="223" height="259" />](http://blog.raffaeu.com/wp-content/uploads/2014/04/image3.png)

First I ask TFS to get an instance of my Workspace, which is the same I am using within Visual Studio. Then, for each project/solution configured in my build definition I update the path. The Workspace name is a Build Parameter in my workflow …

Last piece, we still need to build against a Workspace but the existing one, so in order to accomplish this kind of build we need to change the build path of the local agent in the following way:

[<img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2014/04/image_thumb4.png" width="260" height="220" />](http://blog.raffaeu.com/wp-content/uploads/2014/04/image4.png)

Now when you ask to the workflow to convert Server to Local paths using your Workflow name, it will return a path pointing to the local workspace which is the same path configured in your build agents.

**Note:** Multiple agents can run on the same workflow path in parallel, which means a parallel build sequence <img class="wlEmoticon wlEmoticon-winkingsmile" style="border-top-style: none; border-bottom-style: none; border-right-style: none; border-left-style: none" alt="Winking smile" src="http://blog.raffaeu.com/wp-content/uploads/2014/04/wlEmoticon-winkingsmile.png" />