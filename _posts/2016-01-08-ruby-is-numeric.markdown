---
layout: post
title:  "Ruby: Is Numeric"
date:   2016-01-08 9:45:14 -0600
categories: posts ruby
---

> Note: This is an old post from before my blog died that I rescued via the Wayback Machine.

Sometimes you need to just examine a variable to see if it is a number.  
In duck type languages like Ruby it can be hard to be certain something is a number without examining it.  
In many languages you can have a string that needs to be evaluated to see if it’s a number.  

Here’s how it’s done:  

String mixin for is numeric
{% highlight ruby %}
class String
  def numeric?
    Float(self) != nil rescue false
  end
end
{% endhighlight %}
> Note: Snippet from [mentalized.net](http://mentalized.net/journal/2011/04/14/ruby-how-to-check-if-a-string-is-numeric/) which, in turn, comes from elsewhere.

This attempts to turn the string into a float, returning true if it is successful and false if it is not.
Trying to convert something that can’t become a float into a float causes an ArgumentError to be raised which is then caught by the rescue.
Ruby methods return the value of their last statement so either true will be returned from ‘Float(self) != nil’ or false from the rescue clause.

A quick look at it in action:

Simple Test of numeric? for obvious returns
{% highlight ruby %}
text_string = "I am only text"
text_string.numeric?
# false

text_string = "1359753"
text_string.numeric?
# true
{% endhighlight %}

**Rescue For Flow Control**  
The use of error handling for flow control is considered bad practice but this is the most effective implementation that I’ve seen.

The mentalized.net link above details this a little and discusses using a regular expression to similar, but slower, effect.
I prefer this version as regex can be hard to determine exactly what it is trying to do without closer examination.