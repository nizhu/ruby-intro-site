---
layout: post
title: "Deploying with Heroku"
date: 2013-08-05 22:55
comments: true
categories: [Ruby, SENG2021]
---

Heroku is a pioneer in what we now call Platform-As-A-Service. It's a little on the expensive side to scale on it, but its great for developing with as there is a freebie tier and its service makes deploying to the internet a breeze. Be aware however that if you need a database, PostgreSQL is your only option.

The newcomer in town is [Appfog][6]. They're a lot cheaper than Heroku and supports MySQL in addition to PostgreSQL, but I haven't had the chance to check them out yet.

## Step 1

[Sign up to Heroku][1]

## Step 2

[Install the Heroku Toolbelt][2]

## Step 3

In the root folder of your application, type in the following commands

{% codeblock lang:shell %}
# Initialize a git repository
git init

# Login to heroku (enter your username and password as prompted, generate ssh key if required)
heroku login

# Create a new app on heroku
# Make note of the output of this command
# The http url is where your application will be available - http://<app name>.heroku.com
# The git url is where you will be deploying the application to - git://heroku.com:<app name>.git
# The last line should say 'Git remote heroku added'
# If it doesn't, do 'heroku git:remote -a <app name>'
heroku create

# Add everything to git staging
git add .

# Commit the repository
git commit -m "initial commit"

# Push to Heroku for the first time
git push heroku master

# Ensure that only 1 web dyno will be running
heroku ps:scale web=1
{% endcodeblock %}

## Step 4

Your app should now be available in the http url mentioned in step 3. If there are any problems, you can watch your application's live logging by using ```heroku logs -t ```. As usual, google your issues :) There are some relevant links to documentation below too.

1. [Heroku Signup][1]
2. [Heroku Toolbelt][2]
3. [Heroku Quickstart][3]
4. [Heroku Rails4][4]
5. [Heroku Rails3][8]
6. [Heroku Billing][7]
7. [Appfog Pricing][6]
8. [Demo on Heroku][5]

  [1]: https://id.heroku.com/signup
  [2]: https://toolbelt.heroku.com
  [3]: https://devcenter.heroku.com/articles/quickstart
  [4]: https://devcenter.heroku.com/articles/rails4
  [5]: http://seng2021-rails-demo.herokuapp.com/
  [6]: https://www.appfog.com/pricing/
  [7]: https://devcenter.heroku.com/articles/usage-and-billing#750-free-dyno-hours-per-app
  [8]: https://devcenter.heroku.com/articles/rails3