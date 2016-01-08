---
layout: post
title:  "Ruby: Array Data Types and Nesting"
date:   2016-01-07 17:35:14 -0600
categories: posts ruby
---

> Note: This is an old post from before my blog died that I rescued via the Wayback Machine. It was originally found at: http://code.goingasplannedby.us/2011/ruby-array-data-types-and-nesting/

I recently helped a new Ruby programmer with a question involving multidimensional arrays in ruby.

In case anyone finds this useful, here it is:

The premise was to take an existing array and split a string field into an array at its current location.

This is possible because any position in a ruby array can be any type of object including arrays, hashes, or complex classes.

**Single Depth Array**

We started with a sample array:

{% highlight ruby %}
test_array = [ 1, " my string of text" ]
{% endhighlight %}
By using the .split method we can turn a string into an array of its elements that is broken up, by default, using whitespace as a delimiter.

{% highlight ruby %}
"my string of text".split
# => ["my", "string", "of", "text"]
{% endhighlight %}
Putting this together to edit the sample array in place is simple:

{% highlight ruby %}
test_array = [1 , " my string of text"]
test_array[1] = test_array[1].split

# test_array
#=> ['1',["my","string","of","text"]]
{% endhighlight %}

**Multiple Depth Array**

Doing this across a multidimensional array is still pretty easy and definitely feels more like ruby and less like a C variant.

First we’ll need a sample array:

{% highlight ruby %}
sample_array = [[ 1, " my string of text"], [ 2, "my other test string"]]
{% endhighlight %}
You see we have two entries in this array that follow the same pattern as the original sample.

The first thing we need to do is to examine the individual innermost array.

{% highlight ruby %}
sample_array = [[ 1, " my string of text"], [ 2, "my other test string"]]
sample_array.each do |inner_array|
    # Here we are, looking at each array
    # the first inner_array is [ 1, " my string of text"]
end
{% endhighlight %}
Now that we’re looking at the inner array in a .each loop we could simply refer to position [1] as we did in the single example.

That is very specific against the location and not a very Ruby style approach.

Instead, let’s examine each item and decide how to alter the array based upon what we’re looking at.

{% highlight ruby %}
sample_array = [[ 1, " my string of text"], [ 2, "my other test string"]]
 
sample_array.each do |inner_array|
    inner_array.collect! do |val|
        if val.class == String
            val.split
        else
            val
        end
    end
end
 
# End result: [[1, ["my", "test", "string"]], [2, ["my", "other", "test", "string"]]]
{% endhighlight %}
Lets take a look at that inner section a little at a time.

The .collect! method replaces the array at the current location with the return of the block being executed. Basically this means whatever happens inside the .collect! block alters the array itself. This is how we’re bypassing directly pointing to the array at position [1] and editing it.

The If .class == String evaluation lets us find the strings we want to split and filter out everything else in the else statement.

The interior of the If statement and the Else statement set up the return for the .collect! statement to alter the array with. Ruby returns the output from the last executed code which happens to be either val.split to split the string or simply val to replace the array’s value with itself.

And that’s that.

The problem was resolved and hopefully taught a little ruby with it.