---
id: 39
title: NCache, simple caching of objects.
date: 2012-04-22T20:16:25+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=39
permalink: /archive/2012/04/22/ncache-simple-caching-of-objects.aspx
categories:
  - NCache
  - NET World
---
In the previous post I have introduced you to <a href="http://www.alachisoft.com/ncache/" target="_blank">NCache</a> architecture. In this post we will create our first cache (_locally_) and we will write some C# code to interact with this cache.

For the purpose of this series I have created a very simple WPF 4.0 application available from my SkyDrive (see below at the end of this post) that you can download and run locally; but of course you need to install NCache in your development machine. 

[<img title="SNAGHTML347a43" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="SNAGHTML347a43" src="http://raffaeu.com/wp-content/uploads/2013/03/8a0e9306-c59b-4c93-97e1-7e8b61e0abcbSNAGHTML347a43_thumb.png" width="320" height="221" />](http://raffaeu.com/wp-content/uploads/2013/03/198fb652-2490-43d7-b611-08597e8dcc61SNAGHTML347a43.png)

With this **very simple** WPF application (_even if very simple it works with IoC and MVVM_) I have a Listbox that will provide a list of items (_books titles_). The list will be retrieved with the first command from a database, with the second command from the cache and with the third command the cache will be cleared. The cache is populated **automatically** after you retrieve the data from the database.

**Note:** I am including the full source code of this series but this is not a series about MVVM and/or WPF so I will not provide that code inside the posts in order to stay focus on the topic, which is <a href="http://www.alachisoft.com/ncache/" target="_blank">NCache</a>.

### Create a new local cache

First of all we need to create a new cache in NCache and this can be easily accomplished using the UI of NCache. Open you instance of NCache and create a new **local cache**, which is going to be more than enough for our demo.

The following picture shows the steps you need to follow to create a default local cache in your system.

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/ff470b84-40fc-4c7f-80a9-352bfdb4098dimage_thumb.png" width="420" height="300" />](http://raffaeu.com/wp-content/uploads/2013/03/f12dad50-ab57-492f-b02d-96dc6779c759image_2.png)

  1. Create a new local cache 
  2. Select the current machine as the destination of the local cache 
  3. Name the cache 
  4. Configure the memory allowed by this cache 
  5. Configure the objects lifecycle for the cache 

At this point, NCache will create the new cache and if you have configure it to startup the cache right away, the NCache IDE will open a new window showing you the NCache health status. From this window you can monitor the amount of objects created by the cache and many other functionalities.

### Retrieve the data from the database

In this project I have created a **very simple** database structure that contains a table with a bunch or rows, each row represents a **Book** record. The data is retrieved using Entity Framework and this is the code used to populate the Listbox:

<div id="codeSnippetWrapper" style="cursor: text; font-size: 8pt; border-top: silver 1px solid; font-family: 'Courier New', courier, monospace; border-right: silver 1px solid; width: 97.5%; border-bottom: silver 1px solid; overflow: auto; padding-bottom: 4px; direction: ltr; text-align: left; padding-top: 4px; padding-left: 4px; margin: 20px 0px 10px; border-left: silver 1px solid; line-height: 12pt; padding-right: 4px; max-height: 200px; background-color: #f4f4f4">
  <div id="codeSnippet" style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4">
    <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum1" style="color: #606060">   1:</span> <span style="color: #0000ff">public</span> IList&lt;Book&gt; GetListOfBooks()</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum2" style="color: #606060">   2:</span> {</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum3" style="color: #606060">   3:</span>     <span style="color: #0000ff">using</span> (var orm = <span style="color: #0000ff">new</span> ORM())</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum4" style="color: #606060">   4:</span>     {</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum5" style="color: #606060">   5:</span>         var results = orm.GetListOfBooks();</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum6" style="color: #606060">   6:</span>         <span style="color: #0000ff">return</span> results</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum7" style="color: #606060">   7:</span>             .Select(result =&gt; </pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum8" style="color: #606060">   8:</span>                 <span style="color: #0000ff">new</span> Book {Title = result.Title, ISBN = result.ISBN})</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum9" style="color: #606060">   9:</span>                 .ToList();</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum10" style="color: #606060">  10:</span>     }</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum11" style="color: #606060">  11:</span> }</pre>
    
    <p>
      <!--CRLF--></div> </div> 
      
      <p>
        The data is then <strong>DataBound</strong> to a WPF listbox using the MVVM pattern:
      </p>
      
      <div id="codeSnippetWrapper" style="cursor: text; font-size: 8pt; border-top: silver 1px solid; font-family: 'Courier New', courier, monospace; border-right: silver 1px solid; width: 97.5%; border-bottom: silver 1px solid; overflow: auto; padding-bottom: 4px; direction: ltr; text-align: left; padding-top: 4px; padding-left: 4px; margin: 20px 0px 10px; border-left: silver 1px solid; line-height: 12pt; padding-right: 4px; max-height: 200px; background-color: #f4f4f4">
        <div id="codeSnippet" style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4">
          <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum1" style="color: #606060">   1:</span> <span style="color: #0000ff">&lt;</span><span style="color: #800000">ListBox</span> </pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum2" style="color: #606060">   2:</span>    <span style="color: #ff0000">Grid</span>.<span style="color: #ff0000">Column</span><span style="color: #0000ff">="0"</span> <span style="color: #ff0000">Grid</span>.<span style="color: #ff0000">Row</span><span style="color: #0000ff">="1"</span> <span style="color: #ff0000">Grid</span>.<span style="color: #ff0000">RowSpan</span><span style="color: #0000ff">="3"</span> </pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum3" style="color: #606060">   3:</span>    <span style="color: #ff0000">Style</span><span style="color: #0000ff">="{StaticResource ListboxStyle}"</span> </pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum4" style="color: #606060">   4:</span>    <span style="color: #ff0000">ItemsSource</span><span style="color: #0000ff">="{Binding Books,IsAsync=True}"</span><span style="color: #0000ff">&gt;</span></pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum5" style="color: #606060">   5:</span>     <span style="color: #0000ff">&lt;</span><span style="color: #800000">ListBox.ItemTemplate</span><span style="color: #0000ff">&gt;</span></pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum6" style="color: #606060">   6:</span>         <span style="color: #0000ff">&lt;</span><span style="color: #800000">DataTemplate</span><span style="color: #0000ff">&gt;</span></pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum7" style="color: #606060">   7:</span>             <span style="color: #0000ff">&lt;</span><span style="color: #800000">StackPanel</span><span style="color: #0000ff">&gt;</span></pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum8" style="color: #606060">   8:</span>                 <span style="color: #0000ff">&lt;</span><span style="color: #800000">TextBlock</span> <span style="color: #ff0000">Text</span><span style="color: #0000ff">="{Binding ISBN}"</span> <span style="color: #0000ff">/&gt;</span></pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum9" style="color: #606060">   9:</span>                 <span style="color: #0000ff">&lt;</span><span style="color: #800000">TextBlock</span> <span style="color: #ff0000">Text</span><span style="color: #0000ff">="{Binding Title}"</span> <span style="color: #0000ff">/&gt;</span></pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum10" style="color: #606060">  10:</span>             <span style="color: #0000ff">&lt;/</span><span style="color: #800000">StackPanel</span><span style="color: #0000ff">&gt;</span></pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum11" style="color: #606060">  11:</span>         <span style="color: #0000ff">&lt;/</span><span style="color: #800000">DataTemplate</span><span style="color: #0000ff">&gt;</span></pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum12" style="color: #606060">  12:</span>     <span style="color: #0000ff">&lt;/</span><span style="color: #800000">ListBox.ItemTemplate</span><span style="color: #0000ff">&gt;</span></pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum13" style="color: #606060">  13:</span> <span style="color: #0000ff">&lt;/</span><span style="color: #800000">ListBox</span><span style="color: #0000ff">&gt;</span></pre>
          
          <p>
            <!--CRLF--></div> </div> 
            
            <p>
              <font color="#4d4d4d">And the listbox shows the final results:</font>
            </p>
            
            <p>
              <a href="http://raffaeu.com/wp-content/uploads/2013/03/c70369bd-4a13-4d43-8f18-795287e4b0a5SNAGHTML2369f4c.png"><img title="SNAGHTML2369f4c" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="SNAGHTML2369f4c" src="http://raffaeu.com/wp-content/uploads/2013/03/2eb94718-dba0-46a0-a8c2-c63cac0090d2SNAGHTML2369f4c_thumb.png" width="260" height="154" /></a>
            </p>
            
            <h3>
              Retrieve the data from the cache
            </h3>
            
            <p>
              The second command of the UI is calling the cache, instead of calling the database, to retrieve the data. But, how does it work? I have drawn below a very simple logical flow that my application follows in order to retrieve the data from the cache, <strong>when available</strong>.
            </p>
            
            <p>
              <a href="http://raffaeu.com/wp-content/uploads/2013/03/eab92dd9-aa4d-475e-bc8e-9548b11282f0image_4.png"><img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/2df51b42-9ee1-462e-8f5d-6ad645ac106aimage_thumb_1.png" width="260" height="192" /></a>
            </p>
            
            <p>
              And this is how it works:
            </p>
            
            <div id="codeSnippetWrapper" style="cursor: text; font-size: 8pt; border-top: silver 1px solid; font-family: 'Courier New', courier, monospace; border-right: silver 1px solid; width: 97.5%; border-bottom: silver 1px solid; overflow: auto; padding-bottom: 4px; direction: ltr; text-align: left; padding-top: 4px; padding-left: 4px; margin: 20px 0px 10px; border-left: silver 1px solid; line-height: 12pt; padding-right: 4px; max-height: 200px; background-color: #f4f4f4">
              <div id="codeSnippet" style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4">
                <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum1" style="color: #606060">   1:</span> <span style="color: #008000">// initialize the cache</span></pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum2" style="color: #606060">   2:</span> <span style="color: #0000ff">using</span> (Cache cache = NCache.InitializeCache(<span style="color: #006080">"Cache"</span>))</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum3" style="color: #606060">   3:</span> {</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum4" style="color: #606060">   4:</span>     var list = <span style="color: #0000ff">new</span> List&lt;Book&gt;();</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum5" style="color: #606060">   5:</span>     <span style="color: #008000">// if cache empty, populate the cache</span></pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum6" style="color: #606060">   6:</span>     <span style="color: #0000ff">if</span> (cache.Count == 0)</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum7" style="color: #606060">   7:</span>     {</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum8" style="color: #606060">   8:</span>         IList&lt;Book&gt; books = GetListOfBooks();</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum9" style="color: #606060">   9:</span>         <span style="color: #0000ff">foreach</span> (Book book <span style="color: #0000ff">in</span> books)</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum10" style="color: #606060">  10:</span>         {</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum11" style="color: #606060">  11:</span>             cache.Add(book.ISBN, book);</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum12" style="color: #606060">  12:</span>         }</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: white"><span id="lnum13" style="color: #606060">  13:</span>     }</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="border-top-style: none; font-size: 8pt; font-family: 'Courier New', courier, monospace; width: 100%; border-bottom-style: none; color: black; overflow: visible; padding-bottom: 0px; direction: ltr; text-align: left; padding-top: 0px; border-right-style: none; padding-left: 0px; margin: 0em; border-left-style: none; line-height: 12pt; padding-right: 0px; background-color: #f4f4f4"><span id="lnum14" style="color: #606060">  14:</span> }</pre>
                
                <p>
                  <!--CRLF--></div> </div> 
                  
                  <p>
                    Of course this is just a simple implementation to show you how the NCache mechanism works. It is like working with a simple dictionary of objects if you are not implementing NCache on top of another caching framework.
                  </p>
                  
                  <p>
                    As soon as you add the items to the cache, the NCache management studio will show you the new objects cached in the system:
                  </p>
                  
                  <p>
                    <a href="http://raffaeu.com/wp-content/uploads/2013/03/bd88002f-f761-4418-b189-8c2330f74ec6image_6.png"><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image" src="http://raffaeu.com/wp-content/uploads/2013/03/50ecbdac-0805-4623-b289-d7b12cb7e778image_thumb_2.png" width="260" height="181" /></a>
                  </p>
                  
                  <h3>
                    Few final notes
                  </h3>
                  
                  <ul>
                    <li>
                      First of all you need to orchestrate your application in a way that the cache will be bootstrapped with the application, bootstrapping the cache in a method of the data layer like I did it is absolutely wrong!
                    </li>
                    <li>
                      The objects contained inside the cache, like any other cache system, <strong>must be serializable</strong> otherwise you will receive a nice runtime exception from NCache.
                    </li>
                  </ul>