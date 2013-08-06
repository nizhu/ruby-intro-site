---
layout: post
title: "Hash &amp; Symbols"
date: 2013-07-29 22:46
comments: true
categories: [Ruby, SENG2021]
---

Maps in the data structure sense is referred to in Ruby as Hashes. Map in Ruby is used in its functional programming definition.

### Construction

{% codeblock lang:ruby %}
a = { "a" => "b", 3 => "d" }
# {"a"=>"b", 3=>"d"}
b = Hash("a" => "b", 3 => "d")
# {"a"=>"b", 3=>"d"}

c = {}
d = Hash.new

e = Hash.new(0)
{% endcodeblock %}

### Testing for Equality

{% codeblock lang:ruby %}
a==b
# true
a.equal? b
# false

c==d
# true
c.equal? d
# false
{% endcodeblock %}

### Retrieving from Hash

{% codeblock lang:ruby %}
a["a"]
# "b"
a[3]
# "d"

a[4]
# nil
e[4]
# 0
{% endcodeblock %}

### Adding values to the Hash

{% codeblock lang:ruby %}
a[5] = 6
puts a
# {"a"=>"b", 3=>"d", 5=>6}
{% endcodeblock %}

### Other useful functions

{% codeblock lang:ruby %}
b.clear
puts b
# {}

a.empty?
# false
b.empty?
# true

a.length
# 3

a.keys
# ["a", 3, 5]
a.values
# ["b", "d", 6]
{% endcodeblock %}

### Iterating through the hash

{% codeblock lang:ruby %}
a.each { |k,v| puts "The value for #{k} is #{v}" }
# The value for a is b
# The value for 3 is d
# The value for 5 is 

a.each_key { |k| puts "The value for #{k} is #{a[k]}" }
# The value for a is b
# The value for 3 is d
# The value for 5 is 6

a.each_value { |v| puts v }
# b
# d
# 6

a.each do |k,v|
  puts "The value for #{k} is #{v}"
end
# The value for a is b
# The value for 3 is d
# The value for 5 is 6
{% endcodeblock %}

# Symbols

Fixnum always have the same object_id, and we can tell Ruby to do that to Strings too - it's what we call a Symbol. While a new string will be created every time the same String literal is used, the same symbol will always point to the same object in memory. This also means that Ruby's automatic garbage collection will not recycle symbols.

{% codeblock lang:ruby %}
"abc".object_id
# 70139181776180
"abc".object_id
# 70139181819060

:abc.object_id
# 1024808
:abc.object_id
# 1024808
{% endcodeblock %}

Because of this, Symbols are the preferred way to set and get Hash elements.

[Ruby Cookbook][1] attributes this quote to [Jim Weirich][2]:
> If the contents (the sequence of characters) of the object are important, use a string.
> If the identity of the object is important, use a symbol.

Do this..

{% codeblock lang:ruby %}
h = Hash.new
h[:abc] = "xyz"
puts h[:abc]
{% endcodeblock %}

but don't do this..

{% codeblock lang:ruby %}
h = Hash.new
h["abc"] = :xyz
puts ["abc"]
# xyz
{% endcodeblock %}

  [1]: http://shop.oreilly.com/product/9780596523695.do
  [2]: http://onestepback.org/