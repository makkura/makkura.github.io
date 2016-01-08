---
layout: post
title:  "Ruby: Handling Potential Hang Points With Timeout"
date:   2016-01-07 17:55:14 -0600
categories: posts ruby
---

> Note: This is an old post from before my blog died that I rescued via the Wayback Machine. It was originally found at: http://code.goingasplannedby.us/2011/ruby-handling-potential-hang-points-with-timeout/

Today I encountered a problem where a Net::HTTP post didn’t return.  
When I became aware of it, it had been hanging for two hours already holding up a long running back end process, one that runs for months unsupervised until something goes wrong.

Timeout provided a clean, simple way to handle this issue in case it reoccurs.

Some details on the issue and the usage of Timeout follow.

I didn’t expect this kind of hangup. This process had been connecting to an external server for around five months without an unhandled issue like this.
Normally I get a good response or some kind of bad response from the server being posted to.
This time, I got no response at all; it just didn’t make it back.
Obviously I needed to make my code more robust.

My first thought was to look for some kind of time out variable that the post function could use or to find a similar function that supported a time out value where it would give up executing after a set period if there was no response.
I quickly found that the post function doesn’t take anything like that.

I decided to ask in #ruby (on freenode) and see what thoughts people had as to what approach I should take.
I was immediately directed to Timeout.

I am very pleased that Ruby has a built in timeout method and it is very easy to use.

The main feature of Timeout is that if the content in a block isn’t completed before the given timer is run then an error is raised.

My code ended up looking something like this:

{% highlight ruby %}
# Includes and setup for code not related to Timeout is not included

# Timeout needs to be required
require 'timeout'

rescue
    status = Timeout::timeout(180) do
        @response_plain = http.post(uri.path, @data).body
    end
rescue Timeout::Error => e
    raise "Timer Expired -- Waited too long for response from carrier"
end
{% endhighlight %}

Timeout is dead simple.
The 180 refers to the number of seconds before timing out. I chose a long time out due to this being an external server out of my control that I’ve seen take up to two minutes to send a response back.

If it completes properly status holds whatever the block returned, in this case the same body of the post return (the same as @response_plain, which suggests some rewriting).

If it does not complete in time a Timeout::Error is raised.
In this case it is caught by the rescue clause and somewhat more friendly error message is generated.