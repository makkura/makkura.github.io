---
layout: post
title:  "Ruby: Convert Keys To Symbols At All Depths"
date:   2016-01-07 12:25:14 -0600
categories: ruby
---

> Note: This is an old post from before my blog died that I rescued via the Wayback Machine.

**Problem**: String based hash keys need converting to symbols in a mixed Hash at differing depths  
I recently found a need to convert all the keys in a mixed Hash with varying depth into symbols.  
For illustration, I’ll be using this one:

Complex Hash Sample
{% highlight ruby %}
{
	"recipient" => "Mr. Smith",
	"packages"=> [
	{"weight" => 10},
	{"weight" => 25}
	],
	"address" => {
		"city" => "sacramento",
		"state" => "CA",
		"country" => "United States"
	}
}
{% endhighlight %}

**Existing Solution**: [Hash.symbolize!](http://apidock.com/rails/Hash/symbolize_keys%21)   
I turned to Hash.symbolize_keys! found in ActiveSupport.

{% highlight ruby %}
require 'active_support/core_ext'

{
  "recipient" => "Mr. Smith",
  "packages"=> [
  {"weight" => 10},
  {"weight" => 25}
  ],
  "address" => {
    "city" => "sacramento",
    "state" => "CA",
    "country" => "United States"
  }
}.symbolize_keys!

# {:recipient=>"Mr. Smith", :packages=>[{"weight"=>10}, {"weight"=>25}], :address=>{"city"=>"sacramento", "state"=>"CA", "country"=>"United States"}}

{% endhighlight %}
Glancing over the result, you can see that Hash.symbolize_hash! is only meant for single depth hashes.

**Monkey Patched Solution**  
Using Hash.symbolize_keys! as a basis, I monkey patches the Object class to do what I need.
> Note: Monkey patching, especially monkey patching Object can be dangerous and lead to unintended consequences. Please keep this in mind!

**Ruby 1.8.7**

Symbolize for mixed hash at depth for Ruby 1.8.7

{% highlight ruby %}
class Object
  # Attempts to convert Keys to symbols at all depth levels by inspecting Hashes & Arrays
  def symbolize!
    # Hashes
    self.each_pair do |key, value|
      value.symbolize! if [Array, Hash].include? value.class
      self[(key.to_sym rescue key) || key] = delete(key)
    end if self.class == Hash
    # Arrays
    self.each do |value|
      value.symbolize! if [Array, Hash].include? value.class
    end if self.class == Array
    self
  end
end
{% endhighlight %}

**Ruby 1.9+**

Symbolize for mixed hash at depth for Ruby 1.9 and up

{% highlight ruby %}
class Object
  # Attempts to convert Keys to symbols at all depth levels by inspecting Hashes & Arrays
  def symbolize!
    # Hashes
    clone = self.clone if self.class == Hash
    clone.each_pair do |key, value|
      self[key] = value.symbolize! if [Array, Hash].include? value.class
      self[(key.to_sym rescue key) || key] = self.delete(key)
    end if self.class == Hash
    # Arrays
    self.each do |value|
      value.symbolize! if [Array, Hash].include? value.class
    end if self.class == Array
    self
  end
end
{% endhighlight %}
> Note: Differences between these code snippets are due to Ruby 1.8.7 allowing addition of keys to a Hash while looping. Ruby 1.9+ is safer and does not allow this so we loop through a duplicate hash and edit the original.
A discussion of this feature can be found here.

**Snippet Summary**  
The snippet alters Object so is a wide ranging change.  
Running it will only change the content of Hashes and Arrays but everything will respond_to? it.

We start by inspecting it if it’s a hash.  
We handle the deeper levels of the hash by sending each value to Object.symbolize! if they’re Hashes or Arrays.  
Then we create the symbol key and delete the old key. Hash.delete(key) returns the value of the hash at that key so we set the value to the new symbol as well.

If the object is an Array, we send its value to Object.symbolize! if it is a Hash or Array, the same way we did for hashes.

That’s all there is to it. Recursion does the rest of the work and the hash keys at all depths become symbols.

Example Hash after Object.symbolize!
{% highlight ruby %}

{:recipient=>"Mr. Smith", :address=>{:city=>"sacramento", :state=>"CA", :country=>"United States"}, :packages=>[{:weight=>10}, {:weight=>25}]}
{% endhighlight %}