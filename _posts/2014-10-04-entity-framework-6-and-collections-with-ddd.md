---
id: 653
title: Entity Framework 6 and Collections With DDD
date: 2014-10-04T20:15:00+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=653
permalink: /archive/:year/:month/:day/:title/
categories:
  - 'C#'
  - NET World
tags:
  - entity framework
---
If you start to work with Entity Framework 6 and a real Domain modeled following the SOLID principles and most common known rules of DDD (Domain Driven Design) you will also start to clash with some limits imposed by this ORM.

Let’s start with a classic example of a normal Entity that we define as UserRootAggregate. For this root aggregate we have defined some business rules as following:

  1. A User Entity is a root aggregate 
  2. A User Entity can hold 0 or infinite amount of UserSettings objects 
  3. A UserSetting can be created only within the context of a User root aggregate 
  4. A UserSetting can be modified or deleted only within the context of a User root aggregate 
  5. A UserSetting hold a reference to a parent User 

Based on this normal DDD principles I will create the two following objects:

**A User Entity is a root aggregate**

<pre class="csharpcode"><span class="rem">/// My Root Aggregate</span>
<span class="kwrd">public</span> <span class="kwrd">class</span> User : IRootAggregate
{
   <span class="kwrd">public</span> Guid Id { get; set; }

   <span class="rem">/// A root aggregate can be created</span>
   <span class="kwrd">public</span> User() {  }
}</pre>

**A User Entity can hold 0 or infinite amount of UserSettings**

<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> User : IRootAggregate
{
   <span class="kwrd">public</span> Guid Id { get; set; }
   
   <span class="kwrd">public</span> <span class="kwrd">virtual</span> ICollection&lt;UserSetting&gt; Settings { get; set; }

   <span class="kwrd">public</span> User()
   {
      <span class="kwrd">this</span>.Settings = <span class="kwrd">new</span> HashSet&lt;Setting&gt;();
   }
}</pre>

**A UserSetting can be created or modified or deleted only within the context of a User root aggregate**

<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> UserSetting
{
   <span class="kwrd">public</span> Guid Id { get; set; }
   <span class="kwrd">public</span> <span class="kwrd">string</span> Value { get; set; }
   <span class="kwrd">public</span> User User { get; set; }

   <span class="kwrd">internal</span> UserSetting(User user, <span class="kwrd">string</span> <span class="kwrd">value</span>)
   {
      <span class="kwrd">this</span>.Value = <span class="kwrd">value</span>;
      <span class="kwrd">this</span>.User = user;
   }
}

<span class="rem">/// inside the User class</span>
<span class="kwrd">public</span> <span class="kwrd">void</span> CreateSetting(<span class="kwrd">string</span> <span class="kwrd">value</span>)
{
   var setting = <span class="kwrd">new</span> UserSetting (<span class="kwrd">this</span>, <span class="kwrd">value</span>);
   <span class="kwrd">this</span>.Settings.Add(setting)
}

<span class="kwrd">public</span> <span class="kwrd">void</span> ModifySetting(Guid id, <span class="kwrd">string</span> <span class="kwrd">value</span>)
{
   var setting = <span class="kwrd">this</span>.Settings.First(x =&gt; x.Id == id);
   setting.Value = <span class="kwrd">value</span>;
}

<span class="kwrd">public</span> <span class="kwrd">void</span> DeleteSetting(Guid id)
{
   var setting = <span class="kwrd">this</span>.Settings.First(x =&gt; x.Id == id);
   <span class="kwrd">this</span>.Settings.Remove(setting);
}</pre>

<!--EndFragment--></ol> 

So far so good, Now, considering that we have a Foreign Key between the table UserSetting and the table User we can easily map the relationship with this class:

<pre class="csharpcode"><span class="kwrd">public</span> <span class="kwrd">class</span> PersonSettingMap : EntityTypeConfiguration&lt;PersonSetting&gt;
{
   <span class="kwrd">public</span> PersonSettingMap()
   {
       HasRequired(x =&gt; x.User)
           .WithMany(x =&gt; x.Settings)
           .Map(cfg =&gt; cfg.MapKey(<span class="str">"UserID"</span>))
           .WillCascadeOnDelete(<span class="kwrd">true</span>);
   }
}</pre>

Now below I want to show you the **strange** behavior of Entity Framework 6.

If you Add a child object and save the context Entity Framework will properly generate the INSERT statement:

<pre class="csharpcode"><span class="kwrd">using</span> (DbContext context = <span class="kwrd">new</span> DbContext)
{
   var user = context.Set&lt;User&gt;().First();
   user.CreateSetting(<span class="str">"my value"</span>);

   context.SaveChanges();
}</pre>

If you try to UPDATE a child object, again EF is smart enough and will do the same UPDATE statement you would like to get issued:

<pre class="csharpcode"><span class="kwrd">using</span> (DbContext context = <span class="kwrd">new</span> DbContext)
{
   var user = context.Set&lt;User&gt;()
                     .Include(x =&gt; x.Settings).First();
   var setting = user.Settings.First();
   setting.Value = <span class="str">"new value"</span>;

   context.SaveChanges();
}</pre>

The problem occurs with the DELETE. Actually you would issue this C# statement and think that Entity Framework **like any other ORM does already**, will be smart enough to issue the DELETE statement …

<pre class="csharpcode"><span class="kwrd">using</span> (DbContext context = <span class="kwrd">new</span> DbContext)
{
   var user = context.Set&lt;User&gt;()
                     .Include(x =&gt; x.Settings).First();
   var setting = user.Settings.First();
   user.DeleteSetting(setting.Id);

   context.SaveChanges();
}</pre>

But you will get a nice Exception has below:

> _**System.Data.Entity.Infrastructure.DbUpdateException**: </p> 
> 
> An error occurred while saving entities that do not expose foreign key properties for their relationships. 
> 
> The EntityEntries property will return null because a single entity cannot be identified as the source of the exception. 
> 
> Handling of exceptions while saving can be made easier by exposing foreign key properties in your entity types. 
> 
> See the InnerException for details. &#8212;> 
> 
> System.Data.Entity.Core.UpdateException: _**<u>A relationship from the &#8216;UserSetting_User&#8217; AssociationSet is in the &#8216;Deleted&#8217; state.
              
>   
> </u>**_Given multiplicity constraints, a corresponding &#8216;UserSetting\_User\_Source&#8217; must also in the &#8216;Deleted&#8217; state.</em></blockquote> 
> 
> So this means that EF does not understand that we want to delete the Child object. So inside the scope of our Database Context we have to do this:
> 
> <pre class="csharpcode"><span class="kwrd">using</span> (DbContext context = <span class="kwrd">new</span> DbContext)
{
   var user = context.Set&lt;User&gt;()
                     .Include(x =&gt; x.Settings).First();
   var setting = user.Settings.First();
   user.DeleteSetting(setting);

   <span class="rem">// inform EF</span>
   context.Entry(setting.Id).State = EntityState.Deleted;

   context.SaveChanges();
}</pre>
> 
> I have searched a lot about this problem and actually you can read from the Entity Framework team that this is a **feature** that is still not available for the product:
> 
> [http://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1263145-delete-orphans-support](http://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1263145-delete-orphans-support "http://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1263145-delete-orphans-support")