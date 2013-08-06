---
layout: post
title: "Parsing XML with XMLSimple"
date: 2013-07-31 20:19
comments: true
categories: [Ruby, SENG2021]
---

For the purpose of this exercise, I've downloaded and extracted a zip file from the AEC's site ```ftp://results.aec.gov.au/15508/Detailed/Light/aec-mediafeed-Detailed-Light-15508-20100817220150.zip``` Ruby has an XML parser called REXML in its standard library, but it's known to be very slow - [Some 50 times slower than Nokogiri][1]. I would love to demonstrate Nokogiri, but unfortunately it's more complex to use than XmlSimple. XmlSimple parses the data into a native Ruby hash whereas Nokogiri has its own set of classes.

# Third Party libraries

Third party libraries in Ruby are referred to as gems. Gem is an executable that comes with Ruby. Tell it to install, along with the name of the gem and it will download and install the gem that you want as well as all its dependencies...```gem install xml-simple```

# Let's Parse!

{% codeblock lang:ruby %}
require 'xmlsimple'
xml = File.read 'data/xml/aec-mediafeed-results-detailed-light-15508.xml'
data = XmlSimple.xml_in xml
{% endcodeblock %}

Parsing XML takes a little while, and XmlSimple isn't the most efficient of parsers. If speed is a concern at all, you should definitely look into [Nokogiri][2].

Once it's done we can see what's in there one step at a time.

{% codeblock lang:ruby %}
data.keys
# ["Id", "Created", "SchemaVersion", "EmlVersion", "xmlns", "xmlns:eml", "xmlns:ds", "xmlns:xal", "xmlns:xnl", "xmlns:ts", "xmlns:xs", "xs:schemaLocation", "ManagingAuthority", "MessageLanguage", "MessageGenerator", "Cycle", "Results"]

data.["Results"].keys
# NoMethodError

data.["Results"].class
# Array

data.["Results"][0].class
# Hash

data["Results"][0]["Election"][0]["House"][0]["Contests"][0]["Contest"][0].keys
# ["Projected", "ContestIdentifier", "Enrolment", "FirstPreferences", "TwoCandidatePreferred", "TwoPartyPreferred", "PollingPlaces"]

data["Results"][0]["Election"][0]["House"][0]["Contests"][0]["Contest"][0]["TwoPartyPreferred"][0]["Coalition"][0]["Votes"]
# 0

data["Results"][0]["Election"][0]["House"][0]["Contests"][0]["Contest"][0]["Enrolment"]
# 124215
{% endcodeblock %}

  [1]: http://www.rubyinside.com/ruby-xml-performance-benchmarks-1641.html
  [2]: http://nokogiri.org/