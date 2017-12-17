---
id: 272
title: NHibernate, collection with composite-id.
date: 2009-03-19T17:59:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=272
permalink: /archive/2009/03/19/nhibernate-collection-with-composite-id.aspx
categories:
  - ALT
  - 'C#'
  - NHibernate
---
In the <a href="http://blog.raffaeu.com/archive/2009/03/19/nhibernate-and-the-composite-id.aspx" target="_blank">previous post</a> we saw how to map an entity with a composite-id.

Well but now if you have mapped an entity in this way and you need to create a collection of this entity or maybe you have a related child class that uses this primary keys â€¦ you are in trouble! <img src="http://raffaeu.com/wp-content/uploads/2013/03/b88118b8-6c81-4462-a433-22d3f53fff0csmiley-smile.gif" border="0" alt="Smile" />

The way to solve the problem is very easy. First of a short example to understand what Iâ€™m talking about:

<div id="codeSnippetWrapper" style="border-right: silver 1px solid; padding-right: 4px; border-top: silver 1px solid; padding-left: 4px; font-size: 8pt; padding-bottom: 4px; margin: 20px 0px 10px; overflow: auto; border-left: silver 1px solid; width: 97.5%; cursor: text; direction: ltr; max-height: 200px; line-height: 12pt; padding-top: 4px; border-bottom: silver 1px solid; font-family: 'Courier New', courier, monospace; background-color: #f4f4f4; text-align: left">
  <div id="codeSnippet" style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none">
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum1" style="color: #606060">   1:</span> <span style="color: #0000ff">class</span> Foo {</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum2" style="color: #606060">   2:</span>     <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> name { get; set; }</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum3" style="color: #606060">   3:</span>     <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> lastname { get; set; }</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum4" style="color: #606060">   4:</span>     <span style="color: #0000ff">public</span> IList&lt;childfoo&gt; listofchild { get; set; }</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum5" style="color: #606060">   5:</span> }</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum6" style="color: #606060">   6:</span> <span style="color: #0000ff">class</span> childfoo {</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum7" style="color: #606060">   7:</span>     <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> name { get; set; }</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum8" style="color: #606060">   8:</span>     <span style="color: #0000ff">public</span> <span style="color: #0000ff">string</span> lastname { get; set; }</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum9" style="color: #606060">   9:</span>     <span style="color: #0000ff">public</span> Foo father { get; set; }</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum10" style="color: #606060">  10:</span> }</pre>
    
    <p>
      <!--CRLF--></div> </div> 
      
      <p>
        Now the first class will be mapped in this way if we assume that â€œname and lastname are the primary keysâ€. <em>(Also remember to override <strong>Equals()</strong> and <strong>GetHashCode()</strong>!!).</em>
      </p>
      
      <div id="codeSnippetWrapper" style="border-right: silver 1px solid; padding-right: 4px; border-top: silver 1px solid; padding-left: 4px; font-size: 8pt; padding-bottom: 4px; margin: 20px 0px 10px; overflow: auto; border-left: silver 1px solid; width: 97.5%; cursor: text; direction: ltr; max-height: 200px; line-height: 12pt; padding-top: 4px; border-bottom: silver 1px solid; font-family: 'Courier New', courier, monospace; background-color: #f4f4f4; text-align: left">
        <div id="codeSnippet" style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none">
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum1" style="color: #606060">   1:</span> &lt;<span style="color: #0000ff">class</span> name=<span style="color: #006080">"Foo"</span> table=<span style="color: #006080">"[Foo]"</span> schema=<span style="color: #006080">"xxx"</span>&gt;</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum2" style="color: #606060">   2:</span>   &lt;composite-id&gt;</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum3" style="color: #606060">   3:</span>     &lt;key-property name=<span style="color: #006080">"name"</span>&gt;</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum4" style="color: #606060">   4:</span>       &lt;column name=<span style="color: #006080">"[name]"</span> sql-type=<span style="color: #006080">"varchar"</span> length=<span style="color: #006080">"50"</span> not-<span style="color: #0000ff">null</span>=<span style="color: #006080">"true"</span>/&gt;</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum5" style="color: #606060">   5:</span>     &lt;/key-property&gt;</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum6" style="color: #606060">   6:</span>     &lt;key-property name=<span style="color: #006080">"lastname"</span>&gt;</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum7" style="color: #606060">   7:</span>       &lt;column name=<span style="color: #006080">"[lastname]"</span> sql-type=<span style="color: #006080">"varchar"</span> length=<span style="color: #006080">"50"</span> not-<span style="color: #0000ff">null</span>=<span style="color: #006080">"true"</span>/&gt;</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum8" style="color: #606060">   8:</span>     &lt;/key-property&gt;</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum9" style="color: #606060">   9:</span>   &lt;/composite-id&gt;</pre>
          
          <p>
            <!--CRLF--></div> </div> 
            
            <div id="codeSnippetWrapper" style="border-right: silver 1px solid; padding-right: 4px; border-top: silver 1px solid; padding-left: 4px; font-size: 8pt; padding-bottom: 4px; margin: 20px 0px 10px; overflow: auto; border-left: silver 1px solid; width: 97.5%; cursor: text; direction: ltr; max-height: 200px; line-height: 12pt; padding-top: 4px; border-bottom: silver 1px solid; font-family: 'Courier New', courier, monospace; background-color: #f4f4f4; text-align: left">
              <div id="codeSnippet" style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none">
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum1" style="color: #606060">   1:</span> &lt;bag name=<span style="color: #006080">"listofchild"</span> table=<span style="color: #006080">"[childfoo]"</span> inverse=<span style="color: #006080">"true"</span></pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum2" style="color: #606060">   2:</span>    cascade=<span style="color: #006080">"all-delete-orphan"</span> generic=<span style="color: #006080">"true"</span> lazy=<span style="color: #006080">"true"</span>&gt;</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum3" style="color: #606060">   3:</span>   &lt;key&gt;</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum4" style="color: #606060">   4:</span>     &lt;column name=<span style="color: #006080">"[name]"</span> sql-type=<span style="color: #006080">"varchar"</span>/&gt;</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum5" style="color: #606060">   5:</span>     &lt;column name=<span style="color: #006080">"[lastname]"</span> sql-type=<span style="color: #006080">"varchar"</span>/&gt;</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum6" style="color: #606060">   6:</span>   &lt;/key&gt;</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum7" style="color: #606060">   7:</span>   &lt;one-to-many <span style="color: #0000ff">class</span>=<span style="color: #006080">"Foo"</span> not-found=<span style="color: #006080">"ignore"</span>/&gt;</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum8" style="color: #606060">   8:</span> &lt;/bag&gt;</pre>
                
                <p>
                  <!--CRLF--></div> </div> 
                  
                  <p>
                    Now we can go back to the child mapping file and reference the many-to-one foreign mapping in this simple way:
                  </p>
                  
                  <div id="codeSnippetWrapper" style="border-right: silver 1px solid; padding-right: 4px; border-top: silver 1px solid; padding-left: 4px; font-size: 8pt; padding-bottom: 4px; margin: 20px 0px 10px; overflow: auto; border-left: silver 1px solid; width: 97.5%; cursor: text; direction: ltr; max-height: 200px; line-height: 12pt; padding-top: 4px; border-bottom: silver 1px solid; font-family: 'Courier New', courier, monospace; background-color: #f4f4f4; text-align: left">
                    <div id="codeSnippet" style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none">
                      <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum1" style="color: #606060">   1:</span> <span style="color: #0000ff">&lt;</span><span style="color: #800000">many-to-one</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">="Foo"</span> <span style="color: #ff0000">lazy</span><span style="color: #0000ff">="proxy"</span> <span style="color: #ff0000">not-null</span><span style="color: #0000ff">="false"</span><span style="color: #0000ff">&gt;</span></pre>
                      
                      <p>
                        <!--CRLF-->
                      </p>
                      
                      <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum2" style="color: #606060">   2:</span>   <span style="color: #0000ff">&lt;</span><span style="color: #800000">column</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">="[name]"</span> <span style="color: #ff0000">sql-type</span><span style="color: #0000ff">="varchar"</span><span style="color: #0000ff">/&gt;</span></pre>
                      
                      <p>
                        <!--CRLF-->
                      </p>
                      
                      <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum3" style="color: #606060">   3:</span>   <span style="color: #0000ff">&lt;</span><span style="color: #800000">column</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">="[lastname]"</span> <span style="color: #ff0000">sql-type</span><span style="color: #0000ff">="varchar"</span><span style="color: #0000ff">/&gt;</span></pre>
                      
                      <p>
                        <!--CRLF-->
                      </p>
                      
                      <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum4" style="color: #606060">   4:</span> <span style="color: #0000ff">&lt;/</span><span style="color: #800000">many-to-one</span><span style="color: #0000ff">&gt;</span></pre>
                      
                      <p>
                        <!--CRLF--></div> </div> 
                        
                        <p>
                          So now my repository will be able to give me:
                        </p>
                        
                        <p>
                          <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/NHibernatecollectionwithcompositeid_FD1D/image.png" rel="lightbox"><img title="image" style="border-right: 0px; border-top: 0px; display: inline; border-left: 0px; border-bottom: 0px" height="180" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/NHibernatecollectionwithcompositeid_FD1D/image_thumb.png" width="244" border="0" /></a>
                        </p>
                        
                        <p>
                          And in that moment my repository will go back and execute the sub-select.
                        </p>