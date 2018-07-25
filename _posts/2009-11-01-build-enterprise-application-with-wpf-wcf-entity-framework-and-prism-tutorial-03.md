---
id: 239
title: Build enterprise application with WPF, WCF, Entity Framework and Prism. Tutorial 03.
date: 2009-11-01T09:56:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=239
permalink: /archive/2009/11/01/build-enterprise-application-with-wpf-wcf-entity-framework-and-prism-tutorial-03.aspx
categories:
  - 'C#'
  - Design Pattern
  - WCF
  - WPF
---
<blockquote style="border-bottom: black 1px solid; border-left: black 1px solid; padding-bottom: 5px; background-color: #ffff00; margin: 5px; padding-left: 5px; padding-right: 5px; border-top: black 1px solid; border-right: black 1px solid; padding-top: 5px">
  <p>
    <strong style="color: #ff0000">Update: </strong>source code and documentation are now available on <a href="http://www.codeplex.com" target="_blank">CodePlex</a> at this address: <a title="http://prismtutorial.codeplex.com/" href="http://prismtutorial.codeplex.com/">http://prismtutorial.codeplex.com/</a>
  </p>
</blockquote>

### Value and business validation with Enterprise Library 4.1 and Entity Framework.

In this article I will show you how to validate a domain using the enterprise library 4.1 over the Entity Framework.

First of all I want to talk about the **validation** and the thousands of implementations you can find over the web. In my opinion there are 2 different types of validation. The **value validation** and the **business validation**.

Whatâ€™s the difference? The first, the **value validation** should validate an entity against itâ€™s value content. For example, the entity order should have a validation of type **NotNull** inside itâ€™s **Id** field, or a **LengthValidation** inside itâ€™s order number, in order to reflect the corresponding field in the database table. In this way we will execute a **value validation** in our domain model, **before sending the data** to the Data Access Layer.

The second validation is the **business validation** and it can be accomplished with a lot of different ways. The most common is the **hard coded way** that personally, I donâ€™t really like it. Whatâ€™s a **business validation**? Letâ€™s say we have an order entity and we have a rule that says _â€œif the total order is grater than 1,000 $ apply 10% of discountâ€._ This is a **business rule** that force our entity to change its value after some **business** considerations.

### Available validation framework for NET.

I use for my value validation the [Enterprise Library Validation block](http://www.codeplex.com/entlib) and I have found it really useful. The only problem is that this framework forces you to use the [decorator pattern](http://en.wikipedia.org/wiki/Decorator_pattern), so we **hard code** our entity with the value validation rules. This approach is fine, but only if related to the **values. You can also include the value validation in a separate XML file, and this is the solution we will use later.**

Another interesting framework is the [Validation Framework project on Codeplex](http://www.codeplex.com/ValidationFramework), that similar to the EL, uses decorator and generics to validate our entities.

Very powerful but with an high learning curve is the powerful [SPRING.NET framework](http://www.springframework.net/doc-1.1-M1/reference/html/validation.html), that inside its application blocks has a space also for the validation. What I really like about SPRING.NET is the fact that you will **not hard code** anything because the entire validation is in a separate XML file. But remember that the learning curve is pretty complex because SPRING.NET is a full IoC tool, so everything is under the concept of be â€œInjectedâ€.

Finally, my friend <a href="http://codeclimber.net.nz" target="_blank">Simone Chiaretta</a>, suggested me this open source framework: <a href="http://www.codeplex.com/FluentValidation" target="_blank">Fluent Validation</a>, that has a nice procedural implementation:

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 540px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">using</span> FluentValidation;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3: <span style="color: #0000ff">public</span> <span style="color: #0000ff">class</span> CustomerValidator: AbstractValidator&lt;Customer&gt; {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:   <span style="color: #0000ff">public</span> CustomerValidator() {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:     RuleFor(customer =&gt; customer.Surname).NotEmpty();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:     RuleFor(customer =&gt; customer.Forename).NotEmpty().WithMessage("<span style="color: #8b0000">Please specify a first name</span>");
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:     RuleFor(customer =&gt; customer.Company).NotNull();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:     RuleFor(customer =&gt; customer.Discount).NotEqual(0).When(customer =&gt; customer.HasDiscount);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:     RuleFor(customer =&gt; customer.Address).Length(20, 250);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:     RuleFor(customer =&gt; customer.Postcode).Must(BeAValidPostcode).WithMessage("<span style="color: #8b0000">Please specify a valid postcode</span>");
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11:   }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13:   <span style="color: #0000ff">private</span> <span style="color: #0000ff">bool</span> BeAValidPostcode(<span style="color: #0000ff">string</span> postcode) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 14:     <span style="color: #008000">// custom postcode validating logic goes here</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 15:   }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 16: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 17: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 18: Customer customer = <span style="color: #0000ff">new</span> Customer();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 19: CustomerValidator validator = <span style="color: #0000ff">new</span> CustomerValidator();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 20: ValidationResult results = validator.Validate(customer);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 21: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 22: <span style="color: #0000ff">bool</span> validationSucceeded = results.IsValid;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 23: IList&lt;ValidationFailure&gt; failures = results.Errors;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 24: </pre>


<p>
  The only <strong>real</strong> <strong>business validator</strong> framework for NET that I know and that I use in production is <strong>IBM iLOG for NET</strong> a real powerful engine for business validation that keeps separate your domain model from the business rules. Itâ€™s cool and probably it needs just 10 articles only in order to describe it, so you can have a look at <a href="http://www.ilog.com/products/rulesnet/index.cfm">the official web site</a>. This is a simple screenshot of how it works:
</p>


<p>
  <a href="http://www.ilog.com/products/rulesnet/index.cfm" target="_blank"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image.png" width="260" height="214" /></a> 
</p>


<h3>
  Enterprise Library Validation with Entity Framework.
</h3>


<p>
  The first problem we have using this framework is the way we can apply the decorator pattern on it. Why? Because in Visual Studio the EF model is built through a DSL tool called Entity Framework designer. So each time we change the model, our validation decorations are lost through the way â€¦
</p>


<p>
  So letâ€™s add a reference to the enterprise library validation block in our project and letâ€™s try to understand how we can use them in a productive way.
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image_3.png" rel="lightbox[article]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image_thumb.png" width="260" height="223" /></a> 
</p>


<p>
  The first approach is to overwrite with the <strong>partial attribute</strong> our domain entity is a way like this one:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 540px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> partial <span style="color: #0000ff">class</span> Customer {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">bool</span> IsValid { <span style="color: #0000ff">get</span> { <span style="color: #0000ff">return</span> Validate(); } }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:     <span style="color: #0000ff">private</span> <span style="color: #0000ff">bool</span> Validate() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:         <span style="color: #008000">//validation implementation</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:         <span style="color: #0000ff">return</span> <span style="color: #0000ff">true</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:     }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9: }</pre>


<p>
  and then validate the entity inside the Validate method. Of course because we cannot use the <strong>partial </strong>attribute for a property we need to find a different way to grab our <strong>validation attributes</strong>. From XML?
</p>


<p>
  Good so first of all we need a basic validation class that will validate every single entity using the generics. This solution is the only affordable because the entity generated with EF inherits already from <strong>EntityObject class</strong>.
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 540px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">static</span> <span style="color: #0000ff">class</span> EntityValidator&lt;T&gt; {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">static</span> Dictionary&lt;<span style="color: #0000ff">string</span>,<span style="color: #0000ff">string</span>&gt; ValidationErrors { <span style="color: #0000ff">get</span>; <span style="color: #0000ff">set</span>; }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:     <span style="color: #0000ff">public</span> <span style="color: #0000ff">static</span> <span style="color: #0000ff">bool</span> Validate&lt;T&gt;(T entity) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:         ValidationResults results = Validation.ValidateFromConfiguration&lt;T&gt;(entity);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:         <span style="color: #0000ff">if</span> (results.IsValid) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:             <span style="color: #0000ff">return</span> <span style="color: #0000ff">true</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:         } <span style="color: #0000ff">else</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:             <span style="color: #0000ff">foreach</span> (var error <span style="color: #0000ff">in</span> results) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11:                 ValidationErrors.Add(error.Key, error.Message);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12:             }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13:             <span style="color: #0000ff">return</span> <span style="color: #0000ff">false</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 14:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 15:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 16: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 17: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 18: </pre>


<p>
  Now we can do the validation in this way from the repository, for example:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 540px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">public</span> <span style="color: #0000ff">int</span> Add&lt;T&gt;(T entity) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     ADVConnection context = GetObjectContext();
</pre>


<pre style="background-color: #ffff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     <span style="color: #0000ff">if</span> (EntityValidator&lt;T&gt;.Validate(entity)) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:         context.AddObject(<span style="color: #0000ff">typeof</span>(T).Name, entity);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:         <span style="color: #0000ff">int</span> result = context.SaveChanges();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:         ReleaseObjectContextIfNotReused();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:         <span style="color: #0000ff">return</span> result;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:     }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:     <span style="color: #0000ff">return</span> -1;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11: </pre>


<p>
  An then we can handle that <strong>return â€“1</strong> in a way that we prefer, maybe we can catch it in the â€œpresenterâ€ and interact with the UI. We just need to keep <em>alive</em> the validation errors generated by the method <strong><em>Validate<T></em></strong>.
</p>


<h3>
  Entity Framework mapping file.
</h3>


<p>
  The Entity Framework works similar to NHibernate, so for each business object we have a mapping file. If you open the .edmx file with a right click->open with-> XML editor, you will get an XML representation of your model, similar to this file:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 540px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1: <span style="color: #0000ff">&lt;</span><span style="color: #800000">EntityType</span> <span style="color: #ff0000">Name</span>=<span style="color: #0000ff">"Customer"</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">Key</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">PropertyRef</span> <span style="color: #ff0000">Name</span>=<span style="color: #0000ff">"CustomerID"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">Key</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span>=<span style="color: #0000ff">"CustomerID"</span> <span style="color: #ff0000">Type</span>=<span style="color: #0000ff">"int"</span> <span style="color: #ff0000">Nullable</span>=<span style="color: #0000ff">"false"</span> <span style="color: #ff0000">StoreGeneratedPattern</span>=<span style="color: #0000ff">"Identity"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span>=<span style="color: #0000ff">"NameStyle"</span> <span style="color: #ff0000">Type</span>=<span style="color: #0000ff">"bit"</span> <span style="color: #ff0000">Nullable</span>=<span style="color: #0000ff">"false"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span>=<span style="color: #0000ff">"Title"</span> <span style="color: #ff0000">Type</span>=<span style="color: #0000ff">"nvarchar"</span> <span style="color: #ff0000">MaxLength</span>=<span style="color: #0000ff">"8"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span>=<span style="color: #0000ff">"FirstName"</span> <span style="color: #ff0000">Type</span>=<span style="color: #0000ff">"nvarchar"</span> <span style="color: #ff0000">Nullable</span>=<span style="color: #0000ff">"false"</span> <span style="color: #ff0000">MaxLength</span>=<span style="color: #0000ff">"50"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span>=<span style="color: #0000ff">"MiddleName"</span> <span style="color: #ff0000">Type</span>=<span style="color: #0000ff">"nvarchar"</span> <span style="color: #ff0000">MaxLength</span>=<span style="color: #0000ff">"50"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">Property</span> <span style="color: #ff0000">Name</span>=<span style="color: #0000ff">"LastName"</span> <span style="color: #ff0000">Type</span>=<span style="color: #0000ff">"nvarchar"</span> <span style="color: #ff0000">Nullable</span>=<span style="color: #0000ff">"false"</span> <span style="color: #ff0000">MaxLength</span>=<span style="color: #0000ff">"50"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11: </pre>


<p>
  As you can see, we have already all the rules in place, so we do not need to declare anything. For example, the EF already knows that the <strong>Title field</strong> should be at most, 8 characters long and can be null. Unfortunately, with the version 3 of EF those rules cannot be connected to the Enterprise Library, easily.
</p>


<h3>
  How we can handle this in our static validator using generics?
</h3>


<p>
  Close you Visual Studio solution, and navigate to your <strong>Enterprise Library installation folder</strong>.
</p>


<p>
  Open the .exe called <strong>EntLibConfig.exe. The first screenshot you will se is something like this:</strong>
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image_4.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image_thumb_3.png" width="260" height="173" /></a> 
</p>


<p>
  Click on the file menu and open the app.config file in the layer <strong>PrismTutorial.DataLayer.App.Config</strong>  and you will load the configuration file inside this tool.
</p>


<p>
  Now, click on the <strong>root of your application</strong> and select <u>new validation application block<strong>.</strong></u> The result should something like this:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image_5.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image_thumb_4.png" width="256" height="260" /></a> 
</p>


<p>
  Now we can create a Rule, or a Rule Set and then we can load our domain model and associate those rules with every entity.
</p>


<p>
  In order to load the domain model, choose <strong>new Type</strong>, in the type window, select the PrismTutorial.DataLayer.DomainModel.dll using this button:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image_6.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image_thumb_5.png" width="260" height="93" /></a> 
</p>


<p>
  At this point you can use the search filter of this window to load one entity per time. I started with the <strong>Customer </strong>entity. The final result is this one:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image_7.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image_thumb_6.png" width="260" height="69" /></a> 
</p>


<p>
  Finally, right click on the customer and select <strong>new rule set</strong> and give it a name. In this rule set we will contain all the rules associated with this entity. 
</p>


<p>
  Now, on the ruleset node, choose <strong>select members</strong> and here itâ€™s the magic!!
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image_8.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image_thumb_7.png" width="258" height="260" /></a> 
</p>


<p>
  Finally, you will have now the node <strong>Customer</strong> showing for every loaded field, an additional node. From here you can select any field and add a new <strong>validator</strong>. You can include the message template, the type of validation and so on â€¦
</p>


<p>
  This is my final result:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image_9.png" rel="lightbox[tutorial]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/OctBuildenterpriseapplicationwithWPFWCF_C11A/image_thumb_8.png" width="260" height="156" /></a> 
</p>


<p>
  If you open Visual Studio, you will find the app.config file changed. Of course you <strong>cannot change the app.config file when the solution is open because in my case, I work under source control and the app.config is locked.</strong>
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 540px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1:   <span style="color: #0000ff">&lt;</span><span style="color: #800000">configSections</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">section</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"validation"</span> <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"Microsoft.Practices.EnterpriseLibrary.Validation.Configuration.ValidationSettings, Microsoft.Practices.EnterpriseLibrary.Validation, Version=4.1.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">section</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"dataConfiguration"</span> <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"Microsoft.Practices.EnterpriseLibrary.Data.Configuration.DatabaseSettings, Microsoft.Practices.EnterpriseLibrary.Data, Version=4.1.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:   <span style="color: #0000ff">&lt;/</span><span style="color: #800000">configSections</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5: </pre>


<p>
  And the Customer validation rules.
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 540px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  1:     <span style="color: #0000ff">&lt;</span><span style="color: #800000">type</span> <span style="color: #ff0000">defaultRuleset</span>=<span style="color: #0000ff">"CustomerValidation"</span> <span style="color: #ff0000">assemblyName</span>=<span style="color: #0000ff">"PrismTutorial.DataLayer, Version=1.0.0.0, Culture=neutral, PublicKeyToken=d5210384f83ebb19"</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  2:       <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"PrismTutorial.DataLayer.Customer"</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  3:       <span style="color: #0000ff">&lt;</span><span style="color: #800000">ruleset</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"CustomerValidation"</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  4:         <span style="color: #0000ff">&lt;</span><span style="color: #800000">properties</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  5:           <span style="color: #0000ff">&lt;</span><span style="color: #800000">property</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"CustomerID"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  6:           <span style="color: #0000ff">&lt;</span><span style="color: #800000">property</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"NameStyle"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  7:           <span style="color: #0000ff">&lt;</span><span style="color: #800000">property</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"Title"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  8:           <span style="color: #0000ff">&lt;</span><span style="color: #800000">property</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"FirstName"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px">  9:           <span style="color: #0000ff">&lt;</span><span style="color: #800000">property</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"MiddleName"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 10:           <span style="color: #0000ff">&lt;</span><span style="color: #800000">property</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"LastName"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 11:           <span style="color: #0000ff">&lt;</span><span style="color: #800000">property</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"Suffix"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 12:           <span style="color: #0000ff">&lt;</span><span style="color: #800000">property</span> <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"CompanyName"</span><span style="color: #0000ff">&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 13:             <span style="color: #0000ff">&lt;</span><span style="color: #800000">validator</span> <span style="color: #ff0000">negated</span>=<span style="color: #0000ff">"false"</span> <span style="color: #ff0000">messageTemplate</span>=<span style="color: #0000ff">"Company name cannot be null."</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 14:               <span style="color: #ff0000">messageTemplateResourceName</span>=<span style="color: #0000ff">""</span> <span style="color: #ff0000">messageTemplateResourceType</span>=<span style="color: #0000ff">""</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 15:               <span style="color: #ff0000">tag</span>=<span style="color: #0000ff">""</span> <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"Microsoft.Practices.EnterpriseLibrary.Validation.Validators.NotNullValidator, Microsoft.Practices.EnterpriseLibrary.Validation, Version=4.1.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 16:               <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"Not Null Validator"</span> <span style="color: #0000ff">/&gt;</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 17:             <span style="color: #0000ff">&lt;</span><span style="color: #800000">validator</span> <span style="color: #ff0000">lowerBound</span>=<span style="color: #0000ff">"0"</span> <span style="color: #ff0000">lowerBoundType</span>=<span style="color: #0000ff">"Ignore"</span> <span style="color: #ff0000">upperBound</span>=<span style="color: #0000ff">"100"</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 18:               <span style="color: #ff0000">upperBoundType</span>=<span style="color: #0000ff">"Inclusive"</span> <span style="color: #ff0000">negated</span>=<span style="color: #0000ff">"false"</span> <span style="color: #ff0000">messageTemplate</span>=<span style="color: #0000ff">"Company Name cannot be long more than 100 characters."</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 19:               <span style="color: #ff0000">messageTemplateResourceName</span>=<span style="color: #0000ff">""</span> <span style="color: #ff0000">messageTemplateResourceType</span>=<span style="color: #0000ff">""</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 20:               <span style="color: #ff0000">tag</span>=<span style="color: #0000ff">""</span> <span style="color: #ff0000">type</span>=<span style="color: #0000ff">"Microsoft.Practices.EnterpriseLibrary.Validation.Validators.StringLengthValidator, Microsoft.Practices.EnterpriseLibrary.Validation, Version=4.1.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px"> 21:               <span style="color: #ff0000">name</span>=<span style="color: #0000ff">"String Length Validator"</span> <span style="color: #0000ff">/&gt;</span></pre>


<p>
  Finally I just want to remember you that you can integrate this editor inside Visual Studio 2008 SP1.
</p>


<p>
  Note: With Visual Studio 2010 and the Entity Framework 4.0, this problem is going to be deprecated because the EF 4 allows us to build POCO object like in NHibernate, so we can then use the decorator pattern or any other type of validation pattern.
</p>


<p>
  Tags: <a href="http://technorati.com/tag/Prism" rel="tag">Prism</a> <a href="http://technorati.com/tag/WPF" rel="tag">WPF</a> <a href="http://technorati.com/tag/WCF" rel="tag">WCF</a> <a href="http://technorati.com/tag/Composite application" rel="tag">Composite application</a>
</p>