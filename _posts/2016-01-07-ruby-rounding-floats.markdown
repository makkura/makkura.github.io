---
layout: post
title:  "Ruby: Rounding Floats"
date:   2016-01-07 12:25:14 -0600
categories: ruby
---

> Note: This is an old post from before my blog died that I rescued via the Wayback Machine

Somewhere around 1.9 ruby’s round function was redone to allow for an elegant way to round decimal numbers to a given precision.

For those of us still using 1.8.7 at times, Ruby doesn’t have a really elegant way to round floats to a level of precision because .round only rounds up to the nearest whole number.

The method I prefer to achieve this goes something like this:
{% highlight ruby %}
# 2 decimal precision
(my_float * 100).round.to_f / 100
{% endhighlight %}
This will shift the decimal place over two places, round it to a whole number, and shift it two places back.

To make it handle any level of precision we can convert it into this:

{% highlight ruby %}
# variable level decimal precision
p = 2
(my_float * 10**p).round.to_f / 10**p
{% endhighlight %}
This uses the same idea by raising 10 to a power equal to the precision you want. Two would be 100, three would be 1000, etc.

Finally we can stop repeating ourselves and simply add this to the float class.
I don’t bother with doing this to all numbers.

{% highlight ruby %}
class Float
    def precision(p)
        # Make sure the precision level is actually an integer and > 0
        raise ArgumentError, "#{p} is an invalid precision level. Valid ranges are integers > 0." unless p.class == Fixnum or p < 0
        # Special case for 0 precision so it returns a Fixnum and thus doesn't have a trailing .0
        return self.round if p == 0
        # Standard case  
        return (self * 10**p).round.to_f / 10**p
    end
end
{% endhighlight %}
Do note that this doesn’t allow for negative precision like .round in 1.9.2 does.
This is because I don’t need it and rather have an error than misuse it somehow.
If you need that feature simply change the unless statement on the raise line.

There you have it, easy to use rounding in 1.8.7.
{% highlight ruby %}
test = 10.33333
test.round(2)
#=> 10.33
{% endhighlight %}
After I wrote this I found someone else that previously wrote almost the exact same method:
[http://rubyglasses.blogspot.com/2007/09/float-precision.html](http://rubyglasses.blogspot.com/2007/09/float-precision.html)  
The author covers a broader version that covers more than Floats in their comments.