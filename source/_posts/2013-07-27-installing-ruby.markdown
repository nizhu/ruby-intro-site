---
layout: post
title: "Installing Ruby"
date: 2013-07-27 12:21
comments: true
categories: [Ruby, SENG2021]
---

When I was first starting out in Ruby, this is what gave me the biggest problems. One can install from their operating system's package manager easily enough (likely be installed already with the system), but because of how quickly the Ruby community moves, using system Ruby is generally not a good idea. Every now and then, you'll come across version incompatibility issues and if you need to work on different apps with different versions of Ruby, then you're in a bit of a mess. Not to mention the permission issues.

To get around this, we use a Ruby version manager - one named simply rvm, and another, rbenv.

I will be going through installing Ruby with rbenv on Bash with you simply because I've personally had fewer issues with it integrating with our Bamboo continuous integration environment, but that's a story for another day. You'll no doubt come across problems that I didn't foresee. If it tells you of a missing dependency, just install it. Setting up Ruby was a great exercise in google-fu for me; don't be discouraged if you end up having to reinstall several times.

Please read the OSX comments even if you don't have OSX!

## OSX

If you have a Mac, you're in luck - you have Homebrew. If you don't, just install it with ```ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"```

{% codeblock lang:sh %}
brew update
brew install rbenv
{% endcodeblock %}

Unfortunately, a plugin is required to install Ruby with rbenv

{% codeblock lang:sh %}
brew install ruby-build
{% endcodeblock %}

'Turn on' rbenv
{% codeblock lang:sh %}
if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi
{% endcodeblock %}

Set up bash to always enable rbenv if available
{% codeblock lang:sh %}
echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.profile
{% endcodeblock %}

Add rbenv's executables to your PATH
{% codeblock lang:sh %}
# My rbenv is installed in /usr/loca/opt/rbenv, but yours may be in ~/.rbenv
# Change the line below appropriately
echo 'export PATH=/usr/local/opt/rbenv/shims:$PATH'
{% endcodeblock %}

Finally install Ruby 2.0.0-p247
{% codeblock lang:sh %}
rbenv install 2.0.0-p247
# Command below refreshes rbenv's executables.
# Make sure you run this every time you (un)install a ruby or a gem - more on this later
rbenv rehash
{% endcodeblock %}

## Debian/Ubuntu/Linux Mint

For fun times, install these dependencies
{% codeblock lang:sh %}
apt-get install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison subversion pkg-config
{% endcodeblock %}

Install rbenv + ruby-build
{% codeblock lang:sh %}
git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL -l
mkdir -p ~/.rbenv/plugins
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
rbenv install 2.0.0-p247
rbenv rehash
{% endcodeblock %}

## Keeping legacy code consistent

You may have Ruby scripts before installing rbenv that ran with system Ruby, so it's best to keep it that way. To ensure that this happens, we can make system ruby the default for rbenv.

{% codeblock lang:sh %}
rbenv global system
rbenv version
# system (set by /usr/local/opt/rbenv/version)
{% endcodeblock %}

## Make rbenv use a specific version

Whenever you interact with rbenv, it will look in your current directory for a file named .rbenv-version before falling back to the global default. Try the following:

{% codeblock lang:sh %}
cd /tmp
rbenv version
# system (set by /usr/local/opt/rbenv/version)
echo '2.0.0-p247' > .rbenv-version
# 2.0.0-p247 (set by /tmp/.rbenv-version)
{% endcodeblock %}

The best thing about forcing versions is that you can commit this file to your code repository, and it will ensure that developer on every machine using rbenv will be using the version of Ruby that is intended.

1. [rvm][1]
2. [rbenv][2]
3. [Wikipedia on Continuous Integration][3]
4. [Atlassian Bamboo][4]
5. [Homebrew][5]
6. [Installing rbenv][6]
7. [ruby-build rbenv plugin][7]

  [1]: https://rvm.io/
  [2]: https://github.com/sstephenson/rbenv
  [3]: https://en.wikipedia.org/wiki/Continuous_integration
  [4]: https://www.atlassian.com/software/bamboo
  [5]: http://brew.sh/
  [6]: https://github.com/sstephenson/rbenv#installation
  [7]: https://github.com/sstephenson/ruby-build
