---
layout: post
title:  "Ruby: Dynamic FizzBuzz"
date:   2016-02-05 19:05:00 -0600
categories: posts ruby
---

Reviewing some ruby and poking around the other night, I ended up making a dynamic solution to the basic FizzBuzz program. I found it amusing enough to post here.
I'm posting it as a step by step process to see some of the transition from the simple example to the dynamic one.
If you're just here for the method, look towards the bottom!

**Classic FizzBuzz**  
The normal version of FizzBuzz is phrased like so: "Write a program that prints the numbers from 1 to 100. But for multiples of three print “Fizz” instead of the number and for the multiples of five print “Buzz”. For numbers which are multiples of both three and five print “FizzBuzz”."

An specific implementation of this could look like this:
{% highlight ruby %}
(1..100).each do |i|
  if i % 3 == 0
    print "Fizz"
  elsif i % 5 == 0
    print "Buzz"
  else
    print i
  end
end
{% endhighlight %}

** FizzBuzz As A Method **  
This does the job but you have to edit it if the requirements change.
For instance if you want to change the range to 50 to 200.
Let's make it a little more flexible.

{% highlight ruby %}
def fizzbuzz(start, stop)
  (start..stop).each do |i|
    if i % 3 == 0
      print "Fizz"
    elsif i % 5 == 0
      print "Buzz"
    else
      print i
    end
  end
end
{% endhighlight %}
Now we can call fizzbuzz(1, 100) for the same results as the first example or give it any number range we want such as fizzbuzz(50, 200) with our changed requirements.

**Making A More Dynamic FizzBuzz**
This is a little more flexible but the numbers we're watching for and the text we're printing out is still fixed.
We'll want to make it even more dynamic to handle those as well.

First, let's expand the method to take in what you want to check for and what to print out when we find it.
That's two related data points for each check so it should work well as a hash.
{% highlight ruby %}
	{3 => "Fizz", 5 => "Buzz"}
{% endhighlight %}

Now we can edit the method to take that in so we can use it.
{% highlight ruby %}
def fizzbuzz(start, stop, catch_points = {})
	(start..stop).each do |i|
		#...
	end
end
{% endhighlight %}

Next we have to actually use it in the method.
To do so, we remove the hard coded checks and replace it with dynamic ones.
{% highlight ruby %}
def fizzbuzz(start, stop, catch_points = {})
	(start..stop).each do |i|
		catch_points.each do |watch, text|
			if i % watch == 0
				print text
			end
		end
	end
end
{% endhighlight %}
Now we can call our dynamic fizzbuzz with something like this: fizzbuzz(0,100, { 3 => "Fizz", 5 => "Buzz"}) .
Well that certainly checks for each item in our hash but we've lost our print outs of things that don't match any of them.
We'll have to watch specifically for them so we know if any of them has been printed yet.

{% highlight ruby %}
def fizzbuzz(start, stop, catch_points = {})
	(start..stop).each do |i|
		printed = false
		catch_points.each do |watch, text|
			if i % watch == 0
				print text
				printed = true
			end
		end
		if printed == false
			print i
		end
	end
end
{% endhighlight %}

Now we should get our expected results.
But what if we give it an out of order hash?
fizzbuzz(0, 100, { 3 => "Fizz", 7 => "Pop", 5 => "Buzz"})
Going off the idea that we want things in order so including 7 would mean it should print out after "Fizz" or "Buzz" prints out (if they should), we'll get a few things out of order here.

This part is an easy fix though as we can just sort the hash.
{% highlight ruby %}
def fizzbuzz(start, stop, catch_points = {})
	catch_points.sort
end
{% endhighlight %}

Pass that our sample parameters again and we should get our expected results this time.
Last, let's clean up the code a little to slim it down.
{% highlight ruby %}
def fizzbuzz(start, stop, catch_points = {})
	catch_points.sort
	(start..stop).each do |i|
		printed = false
		catch_points.each do |watch, text|
			(print text; printed = true;) unless i % watch != 0
			end
		end
		print i unless printed
	end
end
{% endhighlight %}

And there we are, a dynamic FizzBuzz solution taken way too far for amusements sake. 