---
layout: post
title: "Hash &amp; Symbols"
date: 2013-07-29 22:46
comments: true
categories: [Ruby, SENG2021]
---

Maps in the data structure sense is referred to in Ruby as Hashes. Map in Ruby is used in its functional programming definition.

### Construction

{% codeblock %}
a = { "a" => "b", 3 => "d" }
# {"a"=>"b", 3=>"d"}
b = Hash("a" => "b", 3 => "d")
# {"a"=>"b", 3=>"d"}

c = {}
d = Hash.new

e = Hash.new(0)
{% endcodeblock %}

### Testing for Equality

{% codeblock %}
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

{% codeblock %}
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

{% codeblock %}
a[5] = 6
puts a
# {"a"=>"b", 3=>"d", 5=>6}
{% endcodeblock %}

### Other useful functions

{% codeblock %}
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

{% codeblock %}
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
