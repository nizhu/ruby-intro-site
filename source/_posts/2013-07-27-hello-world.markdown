---
layout: post
title: "Hello World"
date: 2013-07-27 14:10
comments: true
categories: [Ruby, SENG2021]
---

It's finally time to start coding. 

{% codeblock lang:ruby hello_world.rb %}
puts "Hello World"
{% endcodeblock %}

That's it! Save it as hello_world.rb, exit, and now we can run the file. Note that # is the comment delimiter in Ruby and Shell so I will be using that to give you an idea of the expected result of the line preceding it.

{% codeblock lang:sh %}
ruby hello_world.rb
# Hello World
{% endcodeblock %}

Like shell scripts, you can also run them as executables. Create another file:

{% codeblock lang:ruby hello_world_exe.rb %}
#!/usr/bin/env ruby
puts "Hello World"
{% endcodeblock %}

And give it executable permission, and run it. 

{% codeblock lang:sh %}
chmod +x hello_world_exe.rb
./hello_world_exe.rb
{% endcodeblock %}

Hashbangs will be explained at the end of this post for those curious

## Interactive Ruby Shell

```irb``` is a tool that provides an interface to Ruby. It's a great feature, especially for developers new to Ruby because it allows one to interact with Ruby for instant feedback, and build up to a complex script line by line. Code behaves exactly the same way in irb as it would in the method described above. So open up ```irb``` and write Hello World again....```puts "Hello World"```

You'll notice that after printing Hello World, it also says => nil; this is the 'null' value in Ruby. It's the value that is returned by the function, and in this case, the value returned by ```puts```.

If you want to import files that you've written, there are two ways to do so. A file that is imported in irb is executed just as it would if it had been imported through normal code, and it is the same as if it had been copy and pasted into irb. The following demonstrates the two major difference between the two options so try this out in irb. Whereas ```require``` is your standard 'import this library', ```load``` is more like 'run this bit of code'.

The first is subtle, but important. The argument to ```load``` is a path to the file. A file you ```load``` can be anywhere on the system, it accepts both relative and absolute paths. The argument to ```require``` is just the name of the file, and it will look for that file only in Ruby's PATH. Since 1.9.2 however, your working directory is no longer included in it.

{% codeblock lang:ruby %}
puts $:
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/site_ruby/2.0.0
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/site_ruby/2.0.0/x86_64-linux
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/site_ruby
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/vendor_ruby/2.0.0
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/vendor_ruby/2.0.0/x86_64-linux
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/vendor_ruby
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/2.0.0
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/2.0.0/x86_64-linux
{% endcodeblock %}

To fix this, we start irb with ```irb -I .```

{% codeblock lang:ruby %}
puts $:
# /root/ruby-intro
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/site_ruby/2.0.0
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/site_ruby/2.0.0/x86_64-linux
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/site_ruby
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/vendor_ruby/2.0.0
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/vendor_ruby/2.0.0/x86_64-linux
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/vendor_ruby
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/2.0.0
# /root/.rbenv/versions/2.0.0-p247/lib/ruby/2.0.0/x86_64-linux
{% endcodeblock %}

Let's try again.

{% codeblock lang:ruby %}
load 'hello_world.rb'
# Hello World
# => true
load 'hello_world.rb'
# Hello World
# => true
{% endcodeblock %}

{% codeblock lang:ruby %}
require 'hello_world'
# Hello World
# => true
require 'hello_world'
# => false
{% endcodeblock %}

The other difference is that ```require``` will not import a file more than once.

## Hashbangs

You might have come across this in COMP2041. This is the line at the top of scripts that tells your shell what to execute it with - your shell will simply interpret it as a shell script without it.

{% codeblock lang:sh %}
chmod +x hello_world.rb
./hello_world.rb
# ./hello_world.rb: line 1: puts: command not found
{% endcodeblock %}

### /usr/bin/env

The characters following the exclamation mark is the path of the executable that you want to run the script with.

Since you now have several different versions of Ruby installed, it's best to make the script run with the version that you intend to. rbenv controls the Ruby version in your environment, so that's the best one to use! By ```/usr/bin/env ruby```, the script will be run with the same ruby as typing ruby in the command line. To demonstrate, create the 2 files below, and then enter the following commands (make note of the versions):

{% codeblock lang:ruby version_system.rb %}
#!/usr/bin/ruby
puts RUBY_VERSION
{% endcodeblock %}

{% codeblock lang:ruby version_env.rb %}
#!/usr/bin/env ruby
puts RUBY_VERSION
{% endcodeblock %}

{% codeblock lang:sh %}
ruby -v
# ruby 2.0.0p247 (2013-06-27 revision 41674) [x86_64-darwin12.4.0]
/usr/bin/ruby -v
# ruby 1.8.7 (2012-02-08 patchlevel 358) [universal-darwin12.0]
chmod +x version_env.rb
./version_env.rb
# 2.0.0
chmod +x version_system.rb
./version_system.rb
# 1.8.7
{% endcodeblock %}

1. [Wikipedia on Hashbangs][2]
2. [Wikipedia on irb][1]
3. [Alan Skorkin on Require vs Load][3]

  [1]: http://en.wikipedia.org/wiki/Shebang_(Unix)
  [2]: http://en.wikipedia.org/wiki/Interactive_Ruby_Shell
  [3]: http://www.skorks.com/2009/08/digging-into-a-ruby-installation-require-vs-load/
