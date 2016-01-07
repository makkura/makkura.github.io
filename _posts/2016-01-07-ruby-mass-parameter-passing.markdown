---
layout: post
title:  "Ruby: Mass Parameter Passing"
date:   2016-01-07 12:25:14 -0600
categories: ruby
---

> Note: This is an old post from before my blog died that I rescued via the Wayback Machine. It was originally found at: http://code.goingasplannedby.us/2011/ruby-mass-parameter-passing/

Ever have a method that has a lot of parameters or has optional parameters so you have to structure your input line just right?

For example something like
{% highlight ruby %}
def my_method(name, age, address = "", height, weight)

end
{% endhighlight %}
Normally you’d gather up the variables you want to pass in, confirm some values such as age must be a number, and pass them into the function in order.

What I prefer to do is a technique often referred to as an options hash.
What you do with an options hash collect the data into a hash and pass that into the method instead.
Inside the method, I can then examine what I need and ignore what I don’t.

This has the benefit of moving error checking the data to the method specifically using it rather than checking directly before passing data through.
Also you don’t have to worry about what order you’re passing data in.

The method definition would look more like this:

{% highlight ruby %} 
def my_method(options = {})
    options.each_pair do |key, value|
        # Be sure to inspect each key & value to make sure it conforms to what you expect to be in that field
        #before using it
   end
end
{% endhighlight %}

Now this can generate more work within the method due to moving error checking and expanding out default values away from the definition line but it does make it a lot easier when you don’t have a front end that tells you what your method parameters are.

For a little more info on using option hashes especially for optional parameters take a peek at:
[http://blog.leshill.org/blog/2009/06/10/default-options-with-ruby.html](http://blog.leshill.org/blog/2009/06/10/default-options-with-ruby.html)