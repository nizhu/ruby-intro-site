---
layout: post
title: "Truthiness &amp; Control Flow"
date: 2013-07-28 15:25
comments: true
categories: [Ruby, SENG2011]
---

Ruby uses keywords true and false to represent truthiness and nil to represent a reference that points to nothing (usually some variation of null in other languages).

Like Fixnum from the previous post, these 3 keywords are immediate values.

{% codeblock %}
true.object_id
# 20
true.object_id
# 20

false.object_id
# 0
false.object_id
# 0

nil.object_id
# 8
nil.object_id
# 8
{% endcodeblock %}

# Control Flow

As mentioned in the first post, Ruby was designed to be readable and flexible. I think control flow statements are the highlight. Further, it's important to always remember that false and nil are the only objects that can be 'untruthy' (including 0).

Firstly, we have the very standard if-then-else statements.

{% codeblock %}
t = true

if t
  puts "t is true"
else
  puts "t is false"
end

# t is true

num = 0

if num < 0
  puts "num is negative"
elsif num == 0
  puts "num is zero"
else
  puts "num is positive"
end

# num is zero
{% endcodeblock %}

Then we have unless, which literally means if not. However, please avoid this if you need to use else (there is no secondary guard like elsif or elsunless) - it's only useful in causing confusion.

{% codeblock %}
t = true

unless t
  puts "t is false"
else
  puts "t is true"
end

# t is true
{% endcodeblock %}

A popular way of writing terse simple control flow without sacrificing on readability is to suffix the control to the line itself

{% codeblock %}
puts "true" if true
# true
# => nil

puts "false" unless true
# => nil
{% endcodeblock %}

Finally, we have the C-style single line statements

{% codeblock %}
true ? true : false
# true

false ? true : false
# false
{% endcodeblock %}

And many more complex variations of them. They're just another way of formatting the first option since they Ruby treats ```;``` as a new line. Please never write code like this :)

{% codeblock %}
if true then true else false end
# true

if true; true else false end
# true

num = 0
if num < 0; puts "negative"; puts "lalala" elsif num > 0; puts "positive" else puts "zero" end
# zero

num = -1
if num < 0; puts "negative"; puts "lalala" elsif num > 0; puts "positive" else puts "zero" end
# negative
# lalala

num = 1
if num < 0; puts "negative"; puts "lalala" elsif num > 0; puts "positive" else puts "zero" end
# positive
{% endcodeblock %}