---
layout: post
title:  "Ruby: Combined Test For Nil And Empty"
date:   2016-01-08 9:45:14 -0600
categories: posts ruby
---

> Note: This is an old post from before my blog died that I rescued via the Wayback Machine.

One of the snippets that I have found useful a number of times is the .blank? method.

I originally found it implemented elsewhere but it comes from ActiveSupport.
It is a mixin adding on to the Object class so any object has access to it.

It allows for a single check to see if an object is nil or empty.
Normally you would have to check for nil before checking for empty like so:

{% highlight ruby %}
if address.nil? || address.empty?
{% endhighlight %}
With the .blank? method you can combine them into one check:

{% highlight ruby %}
if address.blank?
{% endhighlight %}
> Note: This example comes straight from the ActiveSupport documentation. I find it fitting since I first started using .blank? for addresses as well.

If you don’t want to include ActiveSupport, you can add blank? to Object quite easily.
Just add the following snippet to your code or an external file to be included in the project:

.blank? verbose version
{% highlight ruby %}
class Object
   def blank?
      if respond_to? :empty?
         empty?
      else
         !self
      end
   end
end
{% endhighlight %}
Or for the concise version:

.blank? concise version
{% highlight ruby %}
class Object
   def blank?
     respond_to?(:empty?) ? empty? : !self
   end
end
{% endhighlight %}