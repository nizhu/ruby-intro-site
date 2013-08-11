---
layout: post
title: "Functions"
date: 2013-07-30 12:55
comments: true
categories: [Ruby, SENG2021]
---

Firstly the syntax. Let's first define a very basic function we can use for this post.

{% codeblock lang:ruby %}
def print_stuff(str1, str2, reverse = false)
  if reverse
    puts "#{str2} and #{str1} received!"
  else
    puts "#{str1} and #{str2} received!"
  end
  str = str1 + str2
end
{% endcodeblock %}

### Calling functions

Brackets around function parameters is optional in Ruby. However, sometimes it's useful to include them regardless for clarity's sake.

{% codeblock lang:ruby %}
print_stuff("First argument", "Second argument")
# First argument and Second argument received!

print_stuff "First argument", "Second argument"
# First argument and Second argument received!
{% endcodeblock %}

Arguments can be made optional by giving them a default value.

{% codeblock lang:ruby %}
print_stuff("First argument", "Second argument", false)
# First argument and Second argument received!

print_stuff("First argument", "Second argument", true)
# Second argument and First argument received!
{% endcodeblock %}

### Return value

As you may have already noticed, ```print_stuff``` does use ```return``` even though it does exist in Ruby and it does exactly what you'd expect. If the return value is not specified, Ruby will return the value returned in the last executed line of the block

{% codeblock lang:ruby %}
def add(a, b)
  a + b
end

puts add(1, 2)
# 3

def add_return(a, b)
  return a + b
end

puts add_return(1,2)
# 3

def add_print(a, b)
  a + b
  print "add successful"
end

puts add_print(1,2)
# add successful
{% endcodeblock %}

Because ```print "add successful"``` returns ```nil```, ```add_print``` returns nil.

# Error Handling

{% codeblock lang:ruby %}
def do_something
  raise "Failed to do something"
end

do_something
# RuntimeError: Failed to do something

begin
  do_something
  puts "Done something"
rescue
  puts "Rescuing from exception"
end
# Rescuing from exception
{% endcodeblock %}

```retry``` will return the cursor to start of the begin block it belongs to

{% codeblock lang:ruby %}
i = 0
begin
  puts "#{i}"
  i += 1
  if i < 2
    do_something
  end
  puts "All done!"
rescue
  retry
end
# 0
# 1
# All done!
{% endcodeblock %}

Also relevant later on, [Closures][1] and [Metaprogramming][2]

  [1]: http://innig.net/software/ruby/closures-in-ruby
  [2]: https://rubymonk.com/learning/books/2-metaprogramming-ruby/chapters/32-introduction-to-metaprogramming/lessons/75-being-meta
