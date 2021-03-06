---
layout: post
title: "Ruby on Rails"
date: 2013-08-04 19:58
comments: true
categories: [Ruby, SENG2021]
---

As I mentioned in the very first post, Rails has been credited with Ruby's sudden, meteoric rise in popularity. It's not the only web application framework for Ruby, but it remains by far the most popular to this day. If you're interested in a career in web development and want to stay away from corporate enterprises, this is a great way to go about it.

Rails emphasises the Model-View-Controller principle, and if you're not familiar with the concept, it's essentially the separation of the data, to the logic of processing the data, and the display of the processed results (links at the bottom for more details). The application I'll be demoing will ignore the Model part however. This is a very simple application will retrieve some data from Twitter, same as the previous post, and display it on a webpage.

As always, start by installing it...```gem install rails```

## Scaffolding

One of the big features of rails is scaffolding - automatically generating common parts of an application. This is the first step of development. Here, we're asking rails to scaffold the project without support for a database.

{% codeblock lang:sh %}
cd ~
rails new twitter_app -O
cd twitter_app
{% endcodeblock %}

The rails generator has created a lot of new files - about 20 in fact - but there aren't that many we need to worry about.

Before you begin editing the files, copy the twitter configuration file from the previous pages to the ```config/initializers``` directory. Files in this directory are run when the server is started.

## Gemfile

This file basically tells the Ruby environment what gems are required to run the the application. Our app only needs the twitter gem so just add that.

{% codeblock lang:ruby Gemfile %}
source 'https://rubygems.org'
gem 'twitter'
gem 'rails', '4.0.0'
{% endcodeblock %}

Source specifies where it should look for the gems if they were to be installed. We can also specify the version of a gem we want in order to ensure that things work as we expect; don't want updates to break the application. Everything else in the file is optional.

## The Twitter controller

Finally on to the logic... ```rails generate controller twitter```.

The code here is almost identical to that from the previous Twitter post. Now, we're just keeping all the tweets in an array instead of just printing it. At the end of the processing, Rails will automatically look for the template ```views/<controller name>/<function name>.*``` unless you specify which HTML template to use with the ```render```. This is displayed within ```views/layouts/application.html.erb```.

{% codeblock lang:ruby app/controllers/twitter_controller.rb %}
class TwitterController < ApplicationController
  
  def proposals
    @tweets = []
    max_id = -1
    for i in (0..1)
      t = Twitter.search("to:justinbieber marry me", :count => 100, :result_type => "recent")
      t.statuses.each do | tweet |
        @tweets.push tweet
      end
      max_id = t.next_results[:max_id]
    end
  end

  def ausvotes
    @tweets = []
    max_id = -1
    for i in (0..1)
      t = Twitter.search("#ausvotes -rt", :count => 100, :result_type => "recent", :max_id => max_id)
      t.statuses.each do | tweet |
        @tweets.push tweet
      end
      max_id = t.next_results[:max_id]
    end
  end
end
{% endcodeblock %}

## The Rails Router

It's advisable that you leave the commented out lines here until you get the hang of routing, just for reference.

{% codeblock lang:ruby config/routes.rb %}
TwitterApp::Application.routes.draw do
  root 'twitter#proposals'
  get 'proposals' => 'twitter#proposals'
  get 'ausvotes' => 'twitter#ausvotes'
end
{% endcodeblock %}

The first line in the block points the root path, ```/``` to the proposals function in the twitter controller. The second and third points ```/proposals``` and ```/ausvotes``` paths to their respective functions in the same controller.

## Justin Bieber Proposals page

If you're familiar with JSP, this is very much the same - HTML mixed in with some code. Here, we're making a simple table but we're creating a row for each tweet.

{% codeblock lang:html app/views/twitter/proposals.html.erb %}
<h1>Last 200 Justin Bieber Proposals</h1>

<table>
  <tr>
    <th>User</th>
    <th>Text</th>
  </tr>
<% @tweets.each do |tweet| %>
  <tr>
    <td><%= tweet.user.name %></td>
    <td><%= tweet.text %></td>
  </tr>
<% end %>
</table>
{% endcodeblock %}

## #ausvotes page

Identical to the above. Do try to refactor this :)

{% codeblock lang:html app/views/twitter/ausvotes.html.erb %}
<h1>Last 200 Tweets with #ausvotes</h1>

<table>
  <tr>
    <th>User</th>
    <th>Text</th>
  </tr>
<% @tweets.each do |tweet| %>
  <tr>
    <td><%= tweet.user.name %></td>
    <td><%= tweet.text %></td>
  </tr>
<% end %>
</table>
{% endcodeblock %}

## Deploying with WEBrick

WEBrick is a very simple Rails web server, perfect for dev testing. Just go to the root directory of the application and type in ```rails server```. WEBrick listens to port 3000 by default, but look carefully at its output if you come across any problems. The JB Proposals page will be available in @ ```http://0.0.0.0:3000``` and ```http://0.0.0.0:3000/proposals``` while the #ausvotes page will be available at ```http://0.0.0.0:3000/ausvotes```.

{% codeblock lang:sh %}
$ rails server
=> Booting WEBrick
=> Rails 4.0.0 application starting in development on http://0.0.0.0:3000
=> Run `rails server -h` for more startup options
=> Ctrl-C to shutdown server
[2013-08-05 22:13:15] INFO  WEBrick 1.3.1
[2013-08-05 22:13:15] INFO  ruby 2.0.0 (2013-06-27) [x86_64-darwin12.4.0]
[2013-08-05 22:13:15] INFO  WEBrick::HTTPServer#start: pid=25428 port=3000
{% endcodeblock %}

## Adding links between the 2 pages

We want the user to be able to easily manuever between the 2 pages so let's put hyperlinks to the 2 pages at the top of every page (just add the 2 anchor bits in the html below).

{% codeblock lang:html app/views/layouts/application.html.erb %}
<!DOCTYPE html>
<html>
<head>
  <title>TwitterApp</title>
  <%= stylesheet_link_tag    "application", media: "all", "data-turbolinks-track" => true %>
  <%= javascript_include_tag "application", "data-turbolinks-track" => true %>
  <%= csrf_meta_tags %>
</head>
<body>
  <a href="/proposals">JB Proposals</a>
  <a href="/ausvotes">#ausvotes</a>
<%= yield %>

</body>
</html>
{% endcodeblock %}

Just refresh the page to take affect. Notice that this change did not require you to restart WEBrick. Some changes, eg. config changes, will require you to do so.

1. [Wikipedia on MVC][1]
2. [Jeff Atwood on MVC][2]

  [1]: http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller
  [2]: http://www.codinghorror.com/blog/2008/05/understanding-model-view-controller.html