---
layout: post
title: "Truthiness &amp; Control Flow"
date: 2013-07-28 15:25
comments: true
categories: [Ruby, SENG2021]
---

Ruby uses keywords true and false to represent truthiness and nil to represent a reference that points to nothing (usually some variation of null in other languages).

Like Fixnum from the previous post, these 3 keywords are immediate values.

{% codeblock lang:ruby %}
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

## Equality Testing

There are 4 ways of testing for equality in Ruby. You'll be using one of them a majority of the time... ```==```. Generally ```==``` represents equality of the values within the object while ```.equal?``` ensures that the 2 objects are one and the same (reference pointer comparison). However, as always, make sure you test your code thoroughly since these are just methods that are easy to override (more on this later). The other two are more obscure and depends much more on the implementation.

{% codeblock lang:ruby %}
"abc" == "abc"
# true
"abc".equal? "abc"
# false

1 == 1
# true
1.equal? 1
# true
1.eql? 1
# true

1.0 == 1
# true
1.0.equal? 1
# false
1.0.eql? 1
# false
{% endcodeblock %}

# Control Flow

As mentioned in the first post, Ruby was designed to be readable and flexible. I think control flow statements are the highlight. Further, it's important to always remember that false and nil are the only objects that can be 'untruthy' (including 0).

Firstly, we have the very standard if-then-else statements.

{% codeblock lang:ruby %}
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

{% codeblock lang:ruby %}
t = true

unless t
  puts "t is false"
else
  puts "t is true"
end

# t is true
{% endcodeblock %}

A popular way of writing terse simple control flow without sacrificing on readability is to suffix the control to the line itself

{% codeblock lang:ruby %}
puts "true" if true
# true
# => nil

puts "false" unless true
# => nil
{% endcodeblock %}

Finally, we have the C-style single line statements

{% codeblock lang:ruby %}
true ? true : false
# true

false ? true : false
# false
{% endcodeblock %}

And many more complex variations of them. They're just another way of formatting the first option since they Ruby treats ```;``` as a new line. Please never write code like this :)

{% codeblock lang:ruby %}
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

1. [Stack Overflow - What's the difference between equal?, eql?, ===, and ==?][1]

  [1]: http://stackoverflow.com/questions/7156955/whats-the-difference-between-equal-eql-and/7157051#7157051
