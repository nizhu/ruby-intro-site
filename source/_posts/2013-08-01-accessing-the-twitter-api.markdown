---
layout: post
title: "Accessing Twitter's API"
date: 2013-08-01 19:25
comments: true
categories: [Ruby, SENG2021]
---

Since mid June this year, Twitter has forced users to use OAuth to authenticate and access its API. You can no longer access its data in a trivial way like the GitHub example before. You must get 4 keys from [Twitter's developer page][5]: ```Consumer key```, ```Consumer secret```, ```Access token``` and ```Access secret``` - don't need to know what they mean yet, but be sure that the 2 secret keys are not shared. OAuth is a real pain. These 4 keys won't give you access. They'll let you get 3 more one use keys which you can then use to access the API.

Fortunately for you as a Ruby user, there are two libraries that will do all the menial work for you. [Twitter][1] is a conveniently named library to access the standard Twitter API (it is not developed by Twitter Inc) while the other, [TweetStream][2] is designed to use Twitter's streaming API (kind of like email pushing on your phone). It's unlikely that you'll need to use the streaming API for this assignment so I won't be showing you TweetStream.

## Twitter Gem

Install the twitter gem with ```gem install twitter``` and make the following config file, replacing the upper case strings with the relevant keys.

{% codeblock lang:ruby twitter_config.rb %}
Twitter.configure do |config|
  config.consumer_key = YOUR_CONSUMER_KEY
  config.consumer_secret = YOUR_CONSUMER_SECRET
  config.oauth_token = YOUR_OAUTH_TOKEN
  config.oauth_token_secret = YOUR_OAUTH_TOKEN_SECRET
end
{% endcodeblock %}

There are some usage examples [here][7], but there's plenty more functionality so do refer to the [documentation][6].

{% codeblock lang:ruby %}
require 'twitter'
load 'twitter_config.rb'
{% endcodeblock %}

Lets find the last person to have proposed to Justin Bieber

{% codeblock lang:ruby %}
result = Twitter.search("to:justinbieber marry me", :count => 1, :result_type => "recent")

result.class
# Twitter::SearchResults

result.statuses[0].user.screen_name
# 4ever_beliebing
{% endcodeblock %}

Just to give another example, I'm going to get the last tweet from each of the users I'm stalking.

{% codeblock lang:ruby %}
f = Twitter.friends
f.class
# Twitter::Cursor
# -- http://rdoc.info/gems/twitter/Twitter/Cursor

f.collection.class
# Array

f.collection[0].class
# Twitter::User
# -- http://rdoc.info/gems/twitter/Twitter/User

f.collection[0].name
# "John Oliver"

f.collection[0].status
f.collection[0].status.class
# Twitter::Tweet
# -- http://rdoc.info/gems/twitter/Twitter/Tweet

f.collection[0].status.text
# "Mugabe is the Harlem Globetrotters of democracy. His winning record is undeniably impressive, but he doesn't really play by the rules."

for user in f.collection.each
  puts "#{user.name} said '#{user.status.text}'"
end
{% endcodeblock %}

Because we don't care about the return value, let's not do this in ```irb```

{% codeblock lang:ruby last_twitter_status.rb %}
#!/usr/bin/env ruby
require 'twitter'
load 'twitter_config.rb'

f = Twitter.friends
for user in f.collection.each
  puts "#{user.name} said '#{user.status.text}'"
end
{% endcodeblock %}

But there's still a problem, I'm following more users than this.

This is because Twitter paginates the results to 20 by default.  So if there is more than 20 records, you'll have to iterate through each page to get all the results.

{% codeblock lang:ruby last_twitter_status_iterated.rb %}
#!/usr/bin/env ruby
require 'twitter'
load 'twitter_config.rb'

cursor = -1 
while cursor != 0 do
  f = Twitter.friends :cursor => cursor
  for user in f.collection.each
    puts "#{user.name} said '#{user.status.text}'"
  end
  cursor = f.next_cursor
end
{% endcodeblock %}

Just to a more relevant example, this is to get the last 200 tweets with #ausvotes excluding retweets. Search results pagination work slightly different to friends - refer to documentation!!

Note that the count parameter refers to the number you want per page, although the maximum is 100.

{% codeblock lang:ruby recent_ausvotes.rb %}
#!/usr/bin/env ruby
require 'twitter'
load 'twitter_config.rb'

max_id = -1
for i in (0..1)
  t = Twitter.search("#ausvotes -rt", :count => 100, :result_type => "recent", :max_id => max_id)
  t.statuses.each do | tweet |
    puts "#{tweet.user.name} said #{tweet.text}"
  end
  max_id = t.next_results[:max_id]
end
{% endcodeblock %}


1. [RubyGem - Twitter][1]
2. [RubyGem - Tweetstream][2]
3. [Twitter API Authentication Documentation][3]
4. [Twitter API][4]
5. [Twitter Developer Apps][5]
6. [RDoc Twitter][6]

  [1]: https://github.com/sferik/twitter
  [2]: https://github.com/tweetstream/tweetstream
  [3]: https://dev.twitter.com/docs/auth
  [4]: https://dev.twitter.com/docs/api/1.1
  [5]: https://dev.twitter.com/apps
  [6]: http://rdoc.info/gems/twitter
  [7]: https://github.com/sferik/twitter#usage-examples
