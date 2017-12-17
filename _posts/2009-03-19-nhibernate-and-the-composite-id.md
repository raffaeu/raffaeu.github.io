---
id: 273
title: NHibernate and the composite-id.
date: 2009-03-19T11:23:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=273
permalink: /archive/2009/03/19/nhibernate-and-the-composite-id.aspx
categories:
  - ALT
  - 'C#'
  - NHibernate
---
Iâ€™m working with the new version (2.0 GA) of NHibernate. The problem I have encountered today is about the **composite-id**. 

Letâ€™s say we have an entity that doesnâ€™t have a personal id field. We donâ€™t want to use the GUID or any other auto id generator class. We **must** implement the primary key logic in the corresponding table in the database. Now, imagine to have this table:

<table cellspacing="0" cellpadding="0" width="400" border="1">
  <tr>
    <td valign="top" width="100">
      Key
    </td>
    
    <td valign="top" width="100">
      Column Name
    </td>
    
    <td valign="top" width="100">
      SQL Type
    </td>
    
    <td valign="top" width="100">
      Not null
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="100">
      yes
    </td>
    
    <td valign="top" width="100">
      Field01
    </td>
    
    <td valign="top" width="100">
      varchar
    </td>
    
    <td valign="top" width="100">
      true
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="100">
      yes
    </td>
    
    <td valign="top" width="100">
      Field02
    </td>
    
    <td valign="top" width="100">
      varchar
    </td>
    
    <td valign="top" width="100">
      true
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="100">
      yes
    </td>
    
    <td valign="top" width="100">
      Field03
    </td>
    
    <td valign="top" width="100">
      varchar
    </td>
    
    <td valign="top" width="100">
      true
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="100">
      yes
    </td>
    
    <td valign="top" width="100">
      Field04
    </td>
    
    <td valign="top" width="100">
      varchar
    </td>
    
    <td valign="top" width="100">
      true
    </td>
  </tr>
</table>

At this point we will have a mapping XML in NHibernate in this way:

<div id="codeSnippetWrapper" style="border-right: silver 1px solid; padding-right: 4px; border-top: silver 1px solid; padding-left: 4px; font-size: 8pt; padding-bottom: 4px; margin: 20px 0px 10px; overflow: auto; border-left: silver 1px solid; width: 97.5%; cursor: text; direction: ltr; max-height: 200px; line-height: 12pt; padding-top: 4px; border-bottom: silver 1px solid; font-family: 'Courier New', courier, monospace; background-color: #f4f4f4; text-align: left">
  <div id="codeSnippet" style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none">
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum1" style="color: #606060">   1:</span> <span style="color: #0000ff">&lt;</span><span style="color: #800000">class</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">="classname"</span> <span style="color: #ff0000">table</span><span style="color: #0000ff">="[tablename]"</span> <span style="color: #ff0000">schema</span><span style="color: #0000ff">="schemaname"</span><span style="color: #0000ff">&gt;</span></pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum2" style="color: #606060">   2:</span>   <span style="color: #0000ff">&lt;</span><span style="color: #800000">composite-id</span><span style="color: #0000ff">&gt;</span></pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum3" style="color: #606060">   3:</span>     ... ...</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum4" style="color: #606060">   4:</span>     <span style="color: #0000ff">&lt;</span><span style="color: #800000">key-property</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">="Field02"</span><span style="color: #0000ff">&gt;</span></pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum5" style="color: #606060">   5:</span>       <span style="color: #0000ff">&lt;</span><span style="color: #800000">column</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">="[Field02]"</span> <span style="color: #ff0000">sql-type</span><span style="color: #0000ff">="varchar"</span> <span style="color: #ff0000">length</span><span style="color: #0000ff">="5"</span> <span style="color: #ff0000">not-null</span><span style="color: #0000ff">="true"</span><span style="color: #0000ff">/&gt;</span></pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum6" style="color: #606060">   6:</span>     <span style="color: #0000ff">&lt;/</span><span style="color: #800000">key-property</span><span style="color: #0000ff">&gt;</span></pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum7" style="color: #606060">   7:</span>     <span style="color: #0000ff">&lt;</span><span style="color: #800000">key-property</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">="Field03"</span><span style="color: #0000ff">&gt;</span></pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum8" style="color: #606060">   8:</span>       <span style="color: #0000ff">&lt;</span><span style="color: #800000">column</span> <span style="color: #ff0000">name</span><span style="color: #0000ff">="[Field03]"</span> <span style="color: #ff0000">sql-type</span><span style="color: #0000ff">="varchar"</span> <span style="color: #ff0000">length</span><span style="color: #0000ff">="5"</span> <span style="color: #ff0000">not-null</span><span style="color: #0000ff">="true"</span><span style="color: #0000ff">/&gt;</span></pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum9" style="color: #606060">   9:</span>     <span style="color: #0000ff">&lt;/</span><span style="color: #800000">key-property</span><span style="color: #0000ff">&gt;</span></pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum10" style="color: #606060">  10:</span>     ... ...</pre>
    
    <p>
      <!--CRLF-->
    </p>
    
    <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum11" style="color: #606060">  11:</span>   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">composite-id</span><span style="color: #0000ff">&gt;</span></pre>
    
    <p>
      <!--CRLF--></div> </div> 
      
      <p>
        If we compile the DAL of our project, we will receive a fancy error:
      </p>
      
      <blockquote>
        <p>
          <em>â€œcomposite-id class must override Equals()â€ </em>
        </p>
        
        <p>
          <em>â€œcomposite-id class must override GetHashCode()â€</em>
        </p>
      </blockquote>
      
      <p>
        The explanation is very simple. We are saying that this class has 3 fields to implement a <strong>comparison</strong> so the NHB must know how you can compare these fields â€¦ and off course the CLR doesnâ€™t know how to do it.
      </p>
      
      <p>
        This is the solution in our entity:
      </p>
      
      <div id="codeSnippetWrapper" style="border-right: silver 1px solid; padding-right: 4px; border-top: silver 1px solid; padding-left: 4px; font-size: 8pt; padding-bottom: 4px; margin: 20px 0px 10px; overflow: auto; border-left: silver 1px solid; width: 97.5%; cursor: text; direction: ltr; max-height: 200px; line-height: 12pt; padding-top: 4px; border-bottom: silver 1px solid; font-family: 'Courier New', courier, monospace; background-color: #f4f4f4; text-align: left">
        <div id="codeSnippet" style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none">
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum1" style="color: #606060">   1:</span> <span style="color: #0000ff">public</span> <span style="color: #0000ff">override</span> <span style="color: #0000ff">bool</span> Equals(<span style="color: #0000ff">object</span> obj) {</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum2" style="color: #606060">   2:</span>     <span style="color: #0000ff">if</span> (obj == <span style="color: #0000ff">null</span>)</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum3" style="color: #606060">   3:</span>         <span style="color: #0000ff">return</span> <span style="color: #0000ff">false</span>;</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum4" style="color: #606060">   4:</span>     MyEntity t = obj <span style="color: #0000ff">as</span> MyEntity;</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum5" style="color: #606060">   5:</span>     <span style="color: #0000ff">if</span> (t == <span style="color: #0000ff">null</span>)</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum6" style="color: #606060">   6:</span>         <span style="color: #0000ff">return</span> <span style="color: #0000ff">false</span>;</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum7" style="color: #606060">   7:</span>     <span style="color: #0000ff">if</span> (<span style="color: #0000ff">this</span>.f1 == t.f1 && <span style="color: #0000ff">this</span>.f2 == t.f2 && <span style="color: #0000ff">this</span>.f3 == t.f3)</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum8" style="color: #606060">   8:</span>         <span style="color: #0000ff">return</span> <span style="color: #0000ff">true</span>;</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum9" style="color: #606060">   9:</span>     <span style="color: #0000ff">else</span></pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum10" style="color: #606060">  10:</span>         <span style="color: #0000ff">return</span> <span style="color: #0000ff">false</span>;</pre>
          
          <p>
            <!--CRLF-->
          </p>
          
          <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum11" style="color: #606060">  11:</span> }</pre>
          
          <p>
            <!--CRLF--></div> </div> 
            
            <div id="codeSnippetWrapper" style="border-right: silver 1px solid; padding-right: 4px; border-top: silver 1px solid; padding-left: 4px; font-size: 8pt; padding-bottom: 4px; margin: 20px 0px 10px; overflow: auto; border-left: silver 1px solid; width: 97.5%; cursor: text; direction: ltr; max-height: 200px; line-height: 12pt; padding-top: 4px; border-bottom: silver 1px solid; font-family: 'Courier New', courier, monospace; background-color: #f4f4f4; text-align: left">
              <div id="codeSnippet" style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none">
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum1" style="color: #606060">   1:</span> <span style="color: #0000ff">public</span> <span style="color: #0000ff">override</span> <span style="color: #0000ff">int</span> GetHashCode() {</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum2" style="color: #606060">   2:</span>     <span style="color: #0000ff">int</span> hash = 13;</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum3" style="color: #606060">   3:</span>     hash = hash +</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum4" style="color: #606060">   4:</span>       (<span style="color: #0000ff">null</span> == <span style="color: #0000ff">this</span>.f1 ? 0 : <span style="color: #0000ff">this</span>.f1.GetHashCode());</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum5" style="color: #606060">   5:</span>     hash = hash +</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum6" style="color: #606060">   6:</span>       (<span style="color: #0000ff">null</span> == <span style="color: #0000ff">this</span>.f2 ? 0 : <span style="color: #0000ff">this</span>.f2.GetHashCode());</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum7" style="color: #606060">   7:</span>     hash = hash +</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum8" style="color: #606060">   8:</span>       (<span style="color: #0000ff">null</span> == <span style="color: #0000ff">this</span>.f3 ? 0 : <span style="color: #0000ff">this</span>.f3.GetHashCode());</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: white; text-align: left; border-bottom-style: none"><span id="lnum9" style="color: #606060">   9:</span>     <span style="color: #0000ff">return</span> hash;</pre>
                
                <p>
                  <!--CRLF-->
                </p>
                
                <pre style="padding-right: 0px; padding-left: 0px; font-size: 8pt; padding-bottom: 0px; margin: 0em; overflow: visible; width: 100%; color: black; direction: ltr; border-top-style: none; line-height: 12pt; padding-top: 0px; font-family: 'Courier New', courier, monospace; border-right-style: none; border-left-style: none; background-color: #f4f4f4; text-align: left; border-bottom-style: none"><span id="lnum10" style="color: #606060">  10:</span> }</pre>
                
                <p>
                  <!--CRLF--></div> </div> 
                  
                  <p>
                    There we go!! Now you have implemented a full version of the method â€œ<strong>Hey is this the entity Iâ€™m going to save or is this entity brand new?!?!</strong>â€
                  </p>
                  
                  <h5>
                    Personal Consideration.
                  </h5>
                  
                  <p>
                    In my opinion NHB should be able to override this method by itself via reflection and not to ask you to rebuild thousand of entities if you want to use the standard pattern <strong>active Record</strong>.
                  </p>