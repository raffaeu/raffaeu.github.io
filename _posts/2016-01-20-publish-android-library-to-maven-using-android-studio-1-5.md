---
id: 718
title: Publish Android library to Maven using Android Studio 1.5
date: 2016-01-20T12:02:46+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=718
permalink: /archive/:year/:month/:day/:title/
categories:
  - Android
---
If you are working with Android Studio and more in general with the Android platform, soon or later you will need to download a library from a Maven or JCenter repository.  
If you are clueless of what I am talking about, just open an Android project using Android Studio and look at the file called **build.gradle (**_The one called build.gradle Project and not the one specific of a module_**)**.

## Gradle dependencies overview

You should see a layout similar to mine:

<pre>buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:1.5.0'
    }
}</pre>

In this file we simply asked Gradle to download the project dependencies from JCenter. This means that Android Studio, when you build the project, will query JCenter central repository and try to resolve any dependency and download them.

Now, if you move through the structure of your Android project you will find another build.grandle file. 

> Actually you will find one per **module**. You can think of a module like a **component** of your android application. 

In this case in my module I have a reference to an external library and I declare the dependency in this way (_at the bottom of my gradle file_):

<pre>dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    <strong>compile 'com.squareup.okhttp:okhttp:2.0.+'</strong>
}</pre>

So, in this example I am working with a library called **okhttp**, available from the package **com.squareup.okhttp** and more precisely I am asking for the version **2.0**. The **+** sign at the end means that any sub-release of the major 2.0 is fine for me, so 2.0.1 or 2.0.999 they are both ok.  
Now, inside my code I can declare this package and start to use its internal classes and interfaces because I know that gradle with synchronize the references into Android Studio during compile time.

Another scenario may happen if you need to work with a public library but the library is not available on Maven Central but on a custom repo. In my case, I have created an upgraded version of a famous library for Android Wear and I do not want to publish it on Maven Central but I rather keep it on my own repo. In this case, in order to use the dependency, **from the module build.gradle** file you must declare first where is located the Maven repository and then you can add the dependency like I did here:

<pre>repositories{
    maven{
        url "http://dl.bintray.com/raffaeu/maven"
    }
}
dependencies {
    compile 'com.mariux.teleport.lib:teleportlib:0.1.6'
}</pre>

If this part is not clear, I personally found very helpful the documentation area of gradle, <a href="https://docs.gradle.org/current/userguide/userguide_single.html" target="_blank">which is available here</a>. 

> _Please pay attention that Android Studio 1.5 works with gradle 1.5 while the latest gradle is now 2.1 so some features may refer to gradle 2.1 which is not compatible with Android Studio_.

## Create an Maven account

Assuming that everything is clear so far now it’s time to deep dive into Maven and create your own account and repository. _Without this part setup you cannot create your own library and publish it_.

Head to <a href="https://bintray.com/" target="_blank">BinTray.com</a> and create a new Account. You can create the new account using Username and Password or you can link one of your existing social accounts: **Google+, GitHub and Twitter**.

[<img title="SNAGHTML14e5a4ba" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="SNAGHTML14e5a4ba" src="http://blog.raffaeu.com/wp-content/uploads/2016/01/SNAGHTML14e5a4ba_thumb.png" width="420" height="231" />](http://blog.raffaeu.com/wp-content/uploads/2016/01/SNAGHTML14e5a4ba.png)

When your account is up and running, you should have an account home page available at this URL: <https://bintray.com/[your_username>].  
In this page you can setup your user profile, change your profile picture and add social accounts.

> **Note:** if you host like me, your open source projects on GitHub, I kindly suggest you to link your GitHub account because it will be a lot easier to display release notes and documentation directly from GitHub.

Now, look at the right pane of your user account and click on the **Maven** link. From there you will be redirected into your Maven’s package manager.

Click **Add New Package** to start to create your public maven library:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2016/01/image_thumb.png" width="244" height="62" />](http://blog.raffaeu.com/wp-content/uploads/2016/01/image.png)

In this page, you have to setup your Maven package information. It is very important how you name your package because this will be the naming convention that we will carry forward on this tutorial, plus it will be used by your users.

From the **owned repositories** choose Maven and a “new Maven package” page will open:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2016/01/image_thumb-1.png" width="244" height="75" />](http://blog.raffaeu.com/wp-content/uploads/2016/01/image-1.png)

### Information regarding your package

In the create new package window, **BinTray** asks you for some basic information required for your package:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2016/01/image_thumb-2.png" width="470" height="291" />](http://blog.raffaeu.com/wp-content/uploads/2016/01/image-2.png)

In my case I am using GitHub so I can easily port my source code repository, my read me files, the issues tracker and the wiki into BinTray.

Now, our package name in BinTray is called **LicenseChecker** but we still do not have any code or library in it, so it’s now time to move into Android Studio 1.5 and create our Package.

## Android Studio and Maven

At this point it’s time to make our Android Library. In my specific case I have a library composed by 3 **modules**, one module refers to a demo app for Smartphone, one module refers to a demo app for Wearable device and one module is my Android Library:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2016/01/image_thumb-3.png" width="260" height="194" />](http://blog.raffaeu.com/wp-content/uploads/2016/01/image-3.png)

### 01 – Preparation

Now, in order to be able to publish the library into BinTray we need to configure Android Studio.

  * Open the build.gradle file related to the project (_the first one in my previous picture_) 
      * Add a reference to the following plugins: 
        <pre>classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'
classpath "com.github.dcendents:android-maven-gradle-plugin:1.3"</pre>
        
          * Re-compile and verify that gradle found your plugins</ul> 
    
    Now your project build.gradle file should look like this one:
    
    <pre>buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:1.5.0'
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'
        classpath "com.github.dcendents:android-maven-gradle-plugin:1.3"
    }
}</pre>
    
    Second step, we need to apply the plug-ins to the libraries that will get published into BinTray. In my case the library project is **licensecheckerlib**, so I am going to edit the build.gradle of this specific module and apply the plug-ins and rebuilt:
    
    <pre>apply plugin: 'com.android.library'
apply plugin: 'com.jfrog.bintray'
apply plugin: 'com.github.dcendents.android-maven'

android {
    compileSdkVersion 23
    buildToolsVersion "23.0.2"</pre>
    
    Now, in order to upload your library, Maven needs information related to the POM file. If you don’t know what is a POM file, I suggest you <a href="https://maven.apache.org/pom.html" target="_blank">to have a look here</a>.
    
    Because we are using the Maven’s plugin for Android Studio, just add these two lines after your plugin declaration (_always inside the library build.gradle file_):
    
    <pre>apply plugin: 'com.android.library'
apply plugin: 'com.jfrog.bintray'
apply plugin: 'com.github.dcendents.android-maven'

group = 'com.raffaeu.licensecheckerlib' // Change this to match your package name
version = '1.0.0' // Change this to match your version number</pre>
    
    Here we are saying to BinTray “_hey, look that I am going to upload a package called com.raffaeu.licensecheckerlib and its version is 1.0.0_”.
    
    Next step, which is optional but mandatory if you are considering to make your library visible over JCenter, Maven Central and more, you need to create a source .jar file. Yes, you need to because the plugin for Maven is capable to build only .aar packages which are not compatibles to JCenter. Always inside your library build.gradle file, create this task:
    
    <pre>task generateSourcesJar(type: Jar) {
    from android.sourceSets.main.java.srcDirs
    classifier 'sources'
}</pre>
    
    Second step to conform with JCenter and Maven Central is to generate also a Java Doc. The JavaDoc is very helpful for your users especially because you are releasing a custom library with custom APIs, so probably the method void doSomething() is unknown to people outside your organization, and this is why JCenter suggests to publish also a Java Doc together with your library.
    
    The JavaDoc should also be transformed into a jar, to do so we create an additional task called **generateJavadocsJar** and we declare a dependency so that the task will not start until the **generateJavadocs** task is completed.
    
    <pre>task generateJavadocs(type: Javadoc) {
    source = android.sourceSets.main.java.srcDirs
    classpath += project.files(android.getBootClasspath()
            .join(File.pathSeparator))
}

task generateJavadocsJar(type: Jar) {
    from generateJavadocs.destinationDir
    classifier 'javadoc'
}

generateJavadocsJar.dependsOn generateJavadocs</pre>
    
    Last step for our preparation is to include the **artifact** keyword of gradle. The artifact keyword is used to inform gradle that a specific library is composed by additional steps, in our case the steps required to generate the .jar and the documentation:
    
    <pre>artifacts {
    archives generateJavadocsJar
    archives generateSourcesJar
}</pre>
    
    At this point we need to build everything and be sure that the tasks are running correctly and that our library is also including documentation and .jar.
    
    Go to Gradle project panel > “refresh” > your library > other > install and double click the task to start it. It will rebuild your library and include also the artifacts required by JCenter:
    
    [<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2016/01/image_thumb-4.png" width="660" height="277" />](http://blog.raffaeu.com/wp-content/uploads/2016/01/image-4.png)
    
    You can double check that everything is done by browsing your library project’s folder and double check that the following items exist:
    
    Your project folder:
    
      * Build > outputs > aar 
          * library-debug.aar 
              * library-release.aar</ul> 
              * Build > libs 
                  * library-1.0.0-javadoc.jar 
                      * library-1.0.0-sources.jar</ul> 
                ### 02 – Publish the library
                
                All right, now we know that our library is building correctly and can be published. This is very important because you can use the **install** task to just rebuild everything and ensure that you are ready to go live. Technically speaking, every time you make a change you should rebuild using install and run your tests. If you get a green light than you are ready to publish into Maven.
                
                In order to publish the artifact into Maven we need to inform the **Maven Plug-in &#8216;com.github.dcendents.android-maven&#8217;** about who we are and what project we are going to upload.  
                The entire documentation for the plugin settings <a href="https://github.com/dcendents/android-maven-gradle-plugin" target="_blank">is available here</a>.**&nbsp;**
                
                <pre>bintray {
    user = '[your BinTray username]'
    key = '[Your bintray key]'
    pkg {
        repo = 'maven'
        name = 'LicenseChecker' // the name of the package in BinTray

        version {
            name = 'licensecheckerlib' // the name of your library project
            desc = 'This is the first version'
            released  = new Date()
            vcsTag = '1.0.0' // the version
        }

        licenses = ['Apache-2.0']
        vcsUrl = 'https://github.com/raffaeu/LicenseChecker.git' // your GitHub repo
        websiteUrl = 'https://github.com/raffaeu/LicenseChecker' // your website or whatever has documentation 
    }
    configurations = ['archives']
}</pre>
                
                Search for the Task **bintrayUpload** and run it:
                
                [<img title="SNAGHTML231544d5" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="SNAGHTML231544d5" src="http://blog.raffaeu.com/wp-content/uploads/2016/01/SNAGHTML231544d5_thumb.png" width="229" height="259" />](http://blog.raffaeu.com/wp-content/uploads/2016/01/SNAGHTML231544d5.png)
                
                At this point you can head to BinTray and **release** your package to the public.
                
                [<img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2016/01/image_thumb-5.png" width="660" height="164" />](http://blog.raffaeu.com/wp-content/uploads/2016/01/image-5.png)
                
                **Note:** Remember that every time you make a new release, BinTray will not publish the package until you confirm that. This is a sort of **safe guard** put in place by BinTray to avoid unwanted publishing.
                
                Last check, before asking BinTray to release your package over Maven and JCenter, you can double check that everything has been published correctly, and in my case here you go:
                
                [<img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; margin: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/2016/01/image_thumb-6.png" width="244" height="202" />](http://blog.raffaeu.com/wp-content/uploads/2016/01/image-6.png)