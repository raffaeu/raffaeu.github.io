---
id: 257
title: WPF and MVVM tutorial 07, the List search.
date: 2009-07-03T16:01:00+00:00
author: raffaeu
layout: post
guid: http://raffaeu.com/?p=257
permalink: /archive/2009/07/03/wpf-and-mvvm-tutorial-07-the-list-search.aspx
categories:
  - 'C#'
  - Design Pattern
  - WPF
---
Ok, starting from this post we are going to do something really interesting. In this episode we need to create a simple List windows, but as you will see the logic will not be so different then a one-to-many form.

This will be the final result:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial07theListsearch_C352/image.png" rel="lightbox[Tutorial07]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial07theListsearch_C352/image_thumb.png" width="260" height="191" /></a> 

The ViewModel for the List Window.

First of all we need to create the view model. The view model should have the following commands:

  1. New â€“ Add a new Customer 
  2. Save â€“ Save all the changes we did â€¦ 
  3. Edit â€“ Edit the current selected customer 
  4. Delete â€“ Delete the current selected customer 
  5. Text box and search button â€“ to operate search activity 
  6. Exit the form 

Then we need to add the following objects:

  1. An observable collection for the list of customers 
  2. A current customer object, but this is not mandatory â€¦ 
  3. The search information we retrieve from the view, or better, the view sends to us. 

The final result will be this view model class:

<a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial07theListsearch_C352/image_3.png" rel="lightbox[Tutorial07]"><img style="border-right-width: 0px; margin: 2px 10px 5px 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" align="left" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial07theListsearch_C352/image_thumb_3.png" width="110" height="260" /></a> 

Letâ€™s have a look at the View Model commands. As you know when we assign a RelayCommand, we can do it in 2 ways:

By passing with a lambda expression the corresponding action.

By passing with a lambda expression the corresponding action and a predicate (something like true/false).

So for some commands like **Save** or **Delete** we can also build a predicate action like **CanSave? CanDelete?** and encapsulating some validation logic inside.

So the code should look like:

Â 

### The RelayCommands.

Simple command like **Create a new customer**:

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #008000">//Private field</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: <span style="color: #0000ff">public</span> ViewCommand newCommand;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: <span style="color: #008000">//Public property to be assigned in the XAML code</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: <span style="color: #0000ff">public</span> ViewCommand NewCommand {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:    <span style="color: #0000ff">get</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:       <span style="color: #0000ff">if</span> (newCommand == <span style="color: #0000ff">null</span>)
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:          newCommand = 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:          <span style="color: #008000">//Lambda expression for assigning the action</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:          <span style="color: #0000ff">new</span> ViewCommand(param =&gt; <span style="color: #0000ff">this</span>.NewCustomer());
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10:       <span style="color: #0000ff">return</span> newCommand;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 11:    }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 12: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 13: <span style="color: #008000">//Real routine executed in the ViewModel</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 14: <span style="color: #0000ff">private</span> <span style="color: #0000ff">void</span> NewCustomer() {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 15:    NavigationActions.OpenCustomerView();
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 16: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 17: </pre>


<p>
  Or something more complex like Delete a customer:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #008000">//Private command field</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: <span style="color: #0000ff">private</span> ViewCommand deleteCommand;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: <span style="color: #008000">//Public databinded ICommand</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: <span style="color: #0000ff">public</span> ViewCommand DeleteCommand {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:    <span style="color: #0000ff">get</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:       <span style="color: #0000ff">if</span> (deleteCommand == <span style="color: #0000ff">null</span>) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:       deleteCommand = <span style="color: #0000ff">new</span> ViewCommand(
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:           param=&gt;<span style="color: #0000ff">this</span>.DeleteCustomer(),
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:           <span style="color: #008000">//Lambda expression for evaluating the execution</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10:           param=&gt;<span style="color: #0000ff">this</span>.CanDeleteCustomer
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 11:           );
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 12:       }   
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 13:       <span style="color: #0000ff">return</span> deleteCommand;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 14:    }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 15: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 16: <span style="color: #008000">//Real delete command</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 17: <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> DeleteCustomer() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 18:    <span style="color: #0000ff">if</span> (SelectedCustomer != <span style="color: #0000ff">null</span>) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 19:       <span style="color: #0000ff">if</span>(NavigationActions.QueryConfirmation(
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 20:          "<span style="color: #8b0000">Delete Customer.</span>",
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 21:          <span style="color: #0000ff">string</span>.Format(<span style="color: #8b0000">"Do you want to delete {0}?"</span>,SelectedCustomer.FirstName))){
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 22:          ListOfCustomers.Remove(SelectedCustomer);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 23:          repository.DeleteCustomer(SelectedCustomer);
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 24:       }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 25:    } <span style="color: #0000ff">else</span> {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 26:       NavigationActions.ShowError(
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 27:         "<span style="color: #8b0000">Delete Customer.</span>", 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 28:         "<span style="color: #8b0000">You must select a Customer!</span>");
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 29:    }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 30: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 31: <span style="color: #008000">//Additional logic can go here ...</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 32: <span style="color: #0000ff">private</span> <span style="color: #0000ff">bool</span> CanDeleteCustomer {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 33:    <span style="color: #0000ff">get</span> { <span style="color: #0000ff">return</span> <span style="color: #0000ff">true</span>; }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 34: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 35: </pre>


<p>
  Then of course we will have two different command for Edit a Customer or create a new one, and at the end the difference will be just here:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #008000">//Open the Customer window empty</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: NavigationActions.OpenCustomerView();

</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: NavigationActions.OpenCustomerView(SelectedCustomer);
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: <span style="color: #008000">//Open the Customer window passing a Customer</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5: </pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6: </pre>


<p>
  The others commands are the same, but if you want in my solution you can find the complete implementation.
</p>


<h3>
  Loading and working with a Collection.
</h3>


<p>
  After we build all the commands and we bind them to the XAML code, we need to load our entities. For this we will use a ObservableCollection List that we will implement in the initialization of our View Model, so when the Window will open we will also load the Collection inside the ListView.
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #008000">//We need an instance of our repository</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: CustomerRepository repository;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: <span style="color: #008000">//This will contain our Customers</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: ObservableCollection&lt;Customer&gt; listOfCustomers;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5: <span style="color: #008000">//This will be the current selected customer</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6: Customer selectedCustomer;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7: <span style="color: #0000ff">public</span> CustomersViewModel() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:    <span style="color: #0000ff">if</span> (repository == <span style="color: #0000ff">null</span>) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:       <span style="color: #008000">//Initialization of the Repository</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10:       repository = <span style="color: #0000ff">new</span> CustomerRepository();
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 11:    }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 12:    Initialization of the List
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 13:    listOfCustomers = 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 14:       <span style="color: #0000ff">new</span> ObservableCollection&lt;Customer&gt;(
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 15:          repository.GetAllCustomers());
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 16:    <span style="color: #0000ff">this</span>.SearchText = "<span style="color: #8b0000">Some text to search ...</span>";
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 17: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 18: <span style="color: #008000">//Binded property containing the Customer list</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 19: <span style="color: #0000ff">public</span> ObservableCollection&lt;Customer&gt; ListOfCustomers {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 20:    <span style="color: #0000ff">get</span> { <span style="color: #0000ff">return</span> listOfCustomers; }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 21: }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 22: </pre>


<h3>
  INotifyPropertyChanged
</h3>


<p>
  If we want to <strong>advise</strong> the UI that we loaded a new customers list, we have to build a ViewModel that implements the INotifyPropertyChanged in this way:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: #region INotifyPropertyChanged Members
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3: <span style="color: #0000ff">public</span> <span style="color: #0000ff">event</span> PropertyChangedEventHandler PropertyChanged;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4: 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5: <span style="color: #0000ff">public</span> <span style="color: #0000ff">void</span> NotifyPropertyChanged(String info) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:    <span style="color: #0000ff">if</span> (PropertyChanged != <span style="color: #0000ff">null</span>) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:       PropertyChanged(
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:          <span style="color: #0000ff">this</span>, 
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:          <span style="color: #0000ff">new</span> PropertyChangedEventArgs(info)
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10:       );
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 11:    }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 12: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 13: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 14: #endregion</pre>


<p>
  And then change the property code in this way:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">public</span> ObservableCollection&lt;Customer&gt; ListOfCustomers {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:     
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:     <span style="color: #0000ff">get</span> { <span style="color: #0000ff">return</span> listOfCustomers; }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:     <span style="color: #0000ff">private</span> <span style="color: #0000ff">set</span> {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:         <span style="color: #0000ff">if</span> (listOfCustomers != <span style="color: #0000ff">value</span>) {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:             listOfCustomers = <span style="color: #0000ff">value</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:             NotifyPropertyChanged("<span style="color: #8b0000">ListOfCustomers</span>");
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:         }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10: }</pre>


<p>
  Now, everytime we are going to load something new into the collection, or we are going to use a <strong>view of the collection</strong> the ViewModel will send a message in the view saying â€œHey, look I changed something in the list, update the UI!!â€.
</p>


<h3>
  Some XAML code.
</h3>


<p>
  Now we have the Commands, the Model and we need to bind them to the view.
</p>


<p>
  Loading the viewmodel into the view:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: ...
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2: <span style="color: #0000ff">&lt;</span><span style="color: #c71585">xmlns</span>:<span style="color: #800000">vm</span>=
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:    "<span style="color: #ff0000">clr</span>-<span style="color: #ff0000">namespace</span>:<span style="color: #ff0000">MVVM</span>.<span style="color: #ff0000">ViewModel</span>;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:    <span style="color: #ff0000">assembly</span>=<span style="color: #0000ff">MVVM.ViewModel</span> /&gt;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5: 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6: ...
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7: &lt;<span style="color: #ff0000">Window</span>.<span style="color: #ff0000">DataContext</span>&gt;
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:    &lt;<span style="color: #ff0000">vm</span>:<span style="color: #ff0000">CustomersViewModel</span>/&gt;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9: &lt;/<span style="color: #ff0000">Window</span>.<span style="color: #ff0000">DataContext</span><span style="color: #0000ff">&gt;</span></pre>


<p>
  Binding one command to a Button:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">&lt;</span><span style="color: #800000">Button</span> <span style="color: #ff0000">Name</span>=<span style="color: #0000ff">"btnNew"</span> <span style="color: #ff0000">Command</span>=<span style="color: #0000ff">"{Binding Path = NewCommand}"</span><span style="color: #0000ff">&gt;</span></pre>


<p>
  Binding the List to the ListView and assigning the selected customer:
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">&lt;</span><span style="color: #800000">ListView</span> 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:   <span style="color: #ff0000">Name</span>=<span style="color: #0000ff">"lstCustomers"</span> 
</pre>


<pre style="background-color: #80ff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:   <span style="color: #ff0000">ItemsSource</span>=<span style="color: #0000ff">"{Binding Path=ListOfCustomers}"</span>
</pre>


<pre style="background-color: #80ff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:   <span style="color: #ff0000">SelectedItem</span>=<span style="color: #0000ff">"{Binding Path=SelectedCustomer}"</span> 
</pre>


<pre style="background-color: #80ff80; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:   <span style="color: #ff0000">SelectionMode</span>=<span style="color: #0000ff">"Single"</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:   <span style="color: #ff0000">IsSynchronizedWithCurrentItem</span>=<span style="color: #0000ff">"True"</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:   <span style="color: #ff0000">HorizontalAlignment</span>=<span style="color: #0000ff">"Stretch"</span> 
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:   <span style="color: #ff0000">VerticalAlignment</span>=<span style="color: #0000ff">"Stretch"</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:   <span style="color: #ff0000">MinHeight</span>=<span style="color: #0000ff">"100"</span><span style="color: #0000ff">&gt;</span></pre>


<p>
  The IsSynchronizedWithCurrentItem bind the listview and the selecteditem together.
</p>


<p>
  Now, what we should be able to do here is to Load the customers, press delete and wipe one or more then one. Then:
</p>


<ol>
  <li>
    If we close the window and we reopen it the Customer will re-appear again. 
  </li>
  
  
  <li>
    If we commit all the changes with the SaveCommand and the we close and re-open the window, the Customer will not be there anymore. 
  </li>
  
</ol>


<p>
  In this lesson we will see only how to implement the search function. In the next one we will see each single command in details.
</p>


<p>
  The user types some text and after that he searches for a result.
</p>


<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 480px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  1: <span style="color: #0000ff">private</span> <span style="color: #0000ff">void</span> FindCustomers() {
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  2:    <span style="color: #008000">//If there is no text, prompt an error</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  3:    <span style="color: #008000">//In the future this will be a XAML trigger</span>
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  4:     <span style="color: #0000ff">if</span> (<span style="color: #0000ff">this</span>.SearchText == <span style="color: #0000ff">string</span>.Empty || <span style="color: #0000ff">this</span>.SearchText == <span style="color: #0000ff">null</span>) {
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  5:         NavigationActions.ShowError("<span style="color: #8b0000">Search Customers.</span>", "<span style="color: #8b0000">Please enter some text ...</span>");
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  6:         <span style="color: #0000ff">return</span>;
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  7:     }
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  8:    <span style="color: #008000">//Keep the search text and build a dynamic query for the DAL</span>
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px">  9:     ListOfCustomers = <span style="color: #0000ff">new</span> ObservableCollection&lt;Customer&gt;(
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 10:         repository.GetCustomersByQuery(
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 11:         p =&gt; p.CompanyName.StartsWith(SearchText)
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 12:         || p.FirstName.StartsWith(SearchText)
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 13:         || p.LastName.StartsWith(SearchText)));
</pre>


<pre style="background-color: #ffffff; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 14: }
</pre>


<pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 13px"> 15: </pre>


<p>
  At this point we will have a first final search solution like these screenshots:
</p>


<p>
  <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial07theListsearch_C352/image_4.png" rel="lightbox[Tutorial07]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; margin-left: 0px; border-left-width: 0px; margin-right: 0px" title="image" border="0" alt="image" align="left" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial07theListsearch_C352/image_thumb_4.png" width="260" height="191" /></a> <a href="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial07theListsearch_C352/image_5.png" rel="lightbox[Tutorial07]"><img style="border-right-width: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px" title="image" border="0" alt="image" align="left" src="http://blog.raffaeu.com/Images/blog_raffaeu_com/WindowsLiveWriter/WPFandMVVMtutorial07theListsearch_C352/image_thumb_5.png" width="260" height="191" /></a> 
</p>


<p>
  <p>
     
  </p>
  
  
  <p>
     
  </p>
  
  
  <p>
     
  </p>
  
  
  <p>
     
  </p>
  
  
  <p>
     
  </p>
  
  
  <p>
    Tags: <a href="http://technorati.com/tag/MVVM" rel="tag">MVVM</a> <a href="http://technorati.com/tag/Model View ViewModel" rel="tag">Model View ViewModel</a> <a href="http://technorati.com/tag/WPF" rel="tag">WPF</a>
  </p>