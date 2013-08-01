---
layout: post
title: "Consuming JSON REST resource through HTTP"
date: 2013-07-31 21:44
comments: true
categories: [Ruby, SENG2021]
---

As you saw in the previous post, XML is an overly verbose format so in the last few years, what we call JSON (JavaScript Object Notation) has come to dominate web communication. Ideally I'd be demonstrating this with Twitter's API since you're likely to use it for this assignment, but since mid June this year, the Twitter API requires OAuth to access. I will be address this in the next post.

If you've been writing Javascript, you love JSON. If you haven't, you soon will. Note that spaces, tabs and line breaks are all optional. I've just added those to make it easier to read.

{% codeblock %}
{
  "id": 32,
  "name": "Mother Duck",
  "children":[
    {
      "id": 12345,
      "name": "Claire"
    },
    {
      "id": 12372,
      "name": "Daniel"
    }
  ],
  "isAlive": true
}
{% endcodeblock %}

That's all there is to JSON, very simple, very clear. ```{}``` encapsulates object/map/hash, while ```[]``` encapsulates an array, both of which can be nested infinitely. Otherwise only strings, numbers, booleans and null are supported values.

## Github API

GitHub is an online source repository, extremely popular with open source projects. You won't be using any data from it for your projects, but it's the first public JSON API that came across my mind. I'm sure you'll be using it somewhere in your career though. For this demonstration, I'll just be using the most simple of calls, retrieving the details of the GitHub user octocat. The documentation for GitHub's API is available [here][2].

The easiest way to access this data, is to simply open a web browser and go to ```https://api.github.com/users/octocat```. Next step further is accessing it through cURL.. ```curl https://api.github.com/users/octocat```. Let's do it in Ruby.

## Retrieving with Net/HTTP and parsing with json

[Net/HTTP][3] is a standard Ruby library, and it'll be your interface to the internet. JSON is a simple parser, but is not standard. As before, install the gem with ```gem install json```

{% codeblock %}
require 'net/http'
require 'json'

uri = URI('https://api.github.com/users/octocat')
response = Net::HTTP.get uri
octocat = JSON.parse response

octocat.keys
# ["login", "id", "avatar_url", "gravatar_id", "url", "html_url", "followers_url", "following_url", "gists_url", "starred_url", "subscriptions_url", "organizations_url", "repos_url", "events_url", "received_events_url", "type", "name", "company", "blog", "location", "email", "hireable", "bio", "public_repos", "followers", "following", "created_at", "updated_at", "public_gists"]

octocat["followers"]
# 398
octocat["public_repos"]
# 3
octocat["hireable"]
# false
{% endcodeblock %}

I had some issues here with openssl on the Mac, but the update instructions with Homebrew found [here][4] worked perfectly.

1. [JSON Examples][1]
2. [Ruby Net/HTTP][3]
3. [Rubygem json][5]

  [1]: http://json.org/example.html
  [2]: http://developer.github.com/v3/
  [3]: http://ruby-doc.org/stdlib-2.0/libdoc/net/http/rdoc/Net/HTTP.html
  [4]: http://railsapps.github.io/openssl-certificate-verify-failed.html
  [5]: http://flori.github.io/json/doc/index.html