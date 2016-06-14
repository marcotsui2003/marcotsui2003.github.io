---
layout: post
title: Building a Simple Web App with Sinatra and Bootstrap
---



Finally I built my first web application using the light weight Ruby framework Sinatra! It is a [book club](https://github.com/marcotsui2003/bookclub) where users share the books they read and what they think about these books. They can view what others are reading, and add those books they like from this list to their own. They can also edit their own book reviews, and remove books from their own reading lists. Secure passwords for user accounts are provided using the Ruby gem [<code class="inline-code">bcrypt</code>](https://github.com/codahale/bcrypt-ruby).

Naturally, the models include <code class="inline-code">Reader</code>, <code class="inline-code">Book</code> and <code class="inline-code">Review</code>. At first, I set up the associations between the model classes as follow: <code class="inline-code">Reader</code> had a <code class="inline-code">has\_many: through:</code> association with <code class="inline-code">Book</code> through a join table class <code class="inline-code">ReaderBook</code>, and <code class="inline-code">Review</code> belongs_to both <code class="inline-code">Reader</code> and <code class="inline-code">Book</code> classes. On second thought I found the <code class="inline-code">ReaderBook</code> class redundant so I dropped it and used <code class="inline-code">Review</code> directly as the join table class. Before the models work perfectly, I needed to tackle a final issue which I will talk about in a future post.

I used the gem [<code class="inline-code">sinatra-flash</code>](https://github.com/SFEley/sinatra-flash) to provide a Rails-like flash system for errors and notice messages:
<br>
{% highlight ruby %}
#BooksController.rb
...
if @reader.save
	    session[:id] = @reader.id
			flash[:notice] = "Account successfully created, welcome #{@reader.username}!"
			redirect '/books'
		else
			flash[:errors] = @reader.errors.full_messages
			redirect '/signup'
		end
...
{% endhighlight ruby %}
<br>
{% highlight erb %}
<!-- flash.erb -->
...
<% if flash[:errors] %>
  <div class="alert alert-danger alert-dismissible" role="alert">
    <button type="button" class="close" data-dismiss="alert" aria-label="Close">
      <span aria-hidden="true">&times;</span>
    </button>
    <% flash[:errors].each do |message| %>
      <%= message %>. <br>
    <% end %>
  </div>
<% end %>
...
{% endhighlight erb %}
<br>
I used [Twitter Bootstrap](http://getbootstrap.com/2.3.2/) for front end. It is very easy to set up in Sinatra. First you need to put the bootstrap css and js files in subfolders in the public folder (default: <code class="inline-code">./public</code>) which serves [static files](http://www.sinatrarb.com/intro.html#Static%20Files). For example, you can put the <code class="inline-code">bootstrap.min.css</code> file in <code class="inline-code">./public/css</code> and <code class="inline-code">bootstrap.min.js</code> in <code class="inline-code">./public/js.</code>, and of course you need to load these files in the the HTML file:
<br>
{% highlight erb %}
<!-- layout.erb -->

<head>
  ...

  <link rel="stylesheet" href="/css/bootstrap.min.css" >
  <link rel="stylesheet" href="/css/cover.css" >
</head>

<body>
  ...

  <script type="text/javascript" src="/js/jquery-1.12.4.js"></script>
  <script type="text/javascript" src="/js/bootstrap.min.js"></script>
</body>

{% endhighlight erb %}

<br>

Below is a youtube demo of this book club app:

<div class="video"><figure><iframe width="720" height="540" src="//www.youtube.com/embed/cFi57rl_fuc" frameborder="0" allowfullscreen></iframe></figure></div>
