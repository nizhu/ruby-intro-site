---
layout: post
title: "Hello World"
date: 2013-07-27 14:10
comments: true
categories: [Ruby, SENG2011]
---

It's finally time to start coding. Start by cloning the git repository I've prepared for this demonstration.

{% codeblock %}
git clone git@github.com:nizhu/ruby-intro.git ~/ruby-intro
cd ~/ruby-intro
ls
{% endcodeblock %}

You'll notice that there are lots of files here, most of which you don't need yet so let's get rid of them for now

{% codeblock %}
git checkout hello-world
ls
{% endcodeblock %}

Now there's only a license file, telling you that you have permission to use all of the code in this repository for whatever purpose, but I will not take any responsibility for any side effects caused by it.... in lawyer-speak, and a README file that doesn't tell you very much.

I promise we're almost there, just open your favourite text editor

{% codeblock lang:ruby hello_world.rb %}
puts "Hello World"
{% endcodeblock %}

That's it! Save it as hello_world.rb, exit, and now we can run the file

{% codeblock %}
ruby hello_world.rb
# Hello World
{% endcodeblock %}

Like shell scripts, you can also run them as executables. Create another file:

{% codeblock lang:ruby hello_world_exe.rb %}
#!/usr/bin/env ruby
puts "Hello World"
{% endcodeblock %}

And give it executable permission, and run it. 

{% codeblock %}
chmod +x hello_world_exe.rb
./hello_world_exe.rb
{% endcodeblock %}

Hashbangs will be explained at the end of this post for those curious

## Interactive Ruby Shell

irb is a tool that provides an interface to Ruby. It's a great feature, especially for developers new to Ruby because it allows one to interact with Ruby for instant feedback, and build up to a complex script line by line. Code behaves exactly the same way in irb as it would in the method described above. So open up irb and write Hello World again.

You'll notice that after printing Hello World, it also says nil. This is the 'null' value in Ruby, but don't worry about why it's doing so for now. This will be explained in the Functions section.

If you want to import files that you've written, there are two ways to do so. A file that is imported in irb is executed just as it would if it had been imported through normal code, and it is the same as if it had been copy and pasted into irb. The following demonstrates the two major difference between the two options so try this out in irb:

{% codeblock %}
load 'hello_world.rb'
# Hello World
# => true
load 'hello_world.rb'
# Hello World
# => true
require 'hello_world'
# Hello World
# => true
require 'hello_world'
# => false
{% endcodeblock %}

The differences are:
* require will not import a file more than once
* file extension is not necessary when importing with require

Generally it's more appropriate to use require unless you do need to execute some code more than once.

## Hashbangs

You might have come across this in COMP2041. This is the line at the top of scripts that tells your shell what to execute it with - your shell will simply interpret it as a shell script without it.

{% codeblock %}
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

{% codeblock %}
ruby -v
# ruby 2.0.0p247 (2013-06-27 revision 41674) [x86_64-darwin12.4.0]
/usr/bin/ruby -v
# ruby 1.8.7 (2012-02-08 patchlevel 358) [universal-darwin12.0]
chmod +x version_env.rb
# 2.0.0
chmod +x version_system.rb
# 1.8.7
{% endcodeblock %}

1. [Wikipedia on Hashbangs][2]
2. [Wikipedia on irb][1]
3. [Alan Skorkin on Require vs Load][3]

  [1]: http://en.wikipedia.org/wiki/Shebang_(Unix)
  [2]: http://en.wikipedia.org/wiki/Interactive_Ruby_Shell
  [3]: http://www.skorks.com/2009/08/digging-into-a-ruby-installation-require-vs-load/
