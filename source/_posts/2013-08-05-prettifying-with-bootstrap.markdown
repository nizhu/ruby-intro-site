---
layout: post
title: "Prettifying with Bootstrap"
date: 2013-08-05 22:27
comments: true
categories: [Ruby, SENG2021]
---

Since Twitter open sourced their CSS framework several years ago, Bootstrap has become the benchmark for CSS. As a terrible designer, I've very much relied on Bootstrap to make my web pages decent looking, not great - just OK.

I've opted for a slightly older version (2.3.2) here since the new version 3 is not yet stable at the time of writing. The documentation for 2.3.2 can be found [here][1]. Regardless, there's only 3 files to change.

{% codeblock lang:html app/views/layouts/application.html.erb %}
<!DOCTYPE html>
<html>
<head>
  <title>TwitterApp</title>
  <link href="//netdna.bootstrapcdn.com/twitter-bootstrap/2.3.2/css/bootstrap-combined.min.css" rel="stylesheet">
  <%= stylesheet_link_tag    "application", media: "all", "data-turbolinks-track" => true %>
  <%= javascript_include_tag "application", "data-turbolinks-track" => true %>
  <%= csrf_meta_tags %>
</head>
<body>
  <div class="container">
    <div class="navbar">
      <div class="navbar-inner">
        <a class="brand" href="/">Twitter App</a>
        <ul class="nav">
          <li><a href="/proposals">JB Proposals</a></li>
          <li><a href="/ausvotes">#ausvotes</a></li>
        </ul>
      </div>
    </div>
<%= yield %>
  </div>
</body>
</html>
{% endcodeblock %}

And add ```class="table"``` to the 2 table tags

{% codeblock lang:html app/views/twitter/ausvotes.html.erb %}
<h1>Last 200 Tweets with #ausvotes</h1>

<table class="table">
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

{% codeblock lang:html app/views/twitter/proposals.html.erb %}
<h1>Last 200 Justin Bieber Proposals</h1>

<table class="table">
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

1. [Twitter Bootstrap][1]

  [1]: http://getbootstrap.com/2.3.2/