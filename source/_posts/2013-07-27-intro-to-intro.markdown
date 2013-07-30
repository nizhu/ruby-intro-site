---
layout: post
title: "Intro to intro"
date: 2013-07-27 10:37
comments: true
categories: [Ruby, SENG2011]
---

Ruby is a dynamic, object oriented programming language that first appeared toward the end of 1995, so it's a bit younger than the other 2 similar, mainstream languages, Python and Perl. In my opinion, it's more hipster and better looking than its cousins, and intuitively, it's as close to English as general purpose programming languages go. As such Ruby is one of the easiest languages to grok.

It really only appeared out of obscurity in 2005 with the rise in popularity of the Ruby-based web application framework, Rails. It has however, greatly matured since then.

There are many different implementations around, including one (JRuby) which compiles your Ruby code to run on the JVM. It allows you to use native Java classes in your Ruby code, and Ruby classes in your Java code. I'm not familiar with the obvious benefits of using these features, but if you ever feel like writing a Swing app with Ruby, you have that option.

The main C-based implementation, also known as MRI, is now up to version 2.0.0-p247. The first version of 2.0.0 was released less than 6 months ago, but the release was designed to be backward compatible with 1.9.3 (with 5 minor exceptions) so one should be able to use any Ruby documentation from the last 6 or so years with little issue.

1. [Official About Ruby][1]
2. [Wikipedia on Ruby][2]
3. [Wikipedia on JRuby][3]
4. [Ruby 2.0.0 Release Notes][4]

  [1]: http://www.ruby-lang.org/en/about/
  [2]: http://en.wikipedia.org/wiki/Ruby_(programming_language)
  [3]: http://en.wikipedia.org/wiki/JRuby
  [4]: http://www.ruby-lang.org/en/news/2013/02/24/ruby-2-0-0-p0-is-released/