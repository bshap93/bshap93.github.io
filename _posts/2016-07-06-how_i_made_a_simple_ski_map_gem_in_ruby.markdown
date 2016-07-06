---
layout: post
title:  "How I Made a Simple Ski Map Gem in Ruby"
date:   2016-07-06 00:51:20 +0000
---


I made my first simple command line gem with Ruby, and published it to RubyGems.org. You can find it <a href="https://rubygems.org/gems/study_the_map">here</a>. To make the gem, I relied on the developer API of skimap.org, and used it to direct users to ski maps. The app allows users to search ski resorts by region, and search for criterion such as regions by letter, and allows the user to pick from a list of years that the maps were made for a given ski resort, and either download, or open it in the browser. One also has the option of searching a world map of ski trails to aid them in finding the right ski map.

<img class="wp-image-119 aligncenter" src="http://www.benjaminlouisshapiro.com/wp-content/uploads/2016/06/Screen-Shot-2016-06-08-at-2.24.00-PM-300x163.png" alt="Screen Shot 2016-06-08 at 2.24.00 PM" width="539" height="293" />

<h2> What I Used </h2>

I started by choosing an idea for a Ruby gem that would be challenging but not overwhelming. I love skiing and studying ski maps, seeing the runs I have skied and mapping out ones I'd like to try. I found skimap.org's API and it seemed straightforward enough, and easy enough for me to use, given my inexperience with APIs. It uses XML, which I was familiar with, and it used basic numeric identifiers for things such as regions, ski areas, and maps. I was able to find pieces of information I wanted and use them in my gem using <a href="http://www.nokogiri.org/">Nokogiri's</a> XML class.

<img class="wp-image-114 aligncenter" src="http://www.benjaminlouisshapiro.com/wp-content/uploads/2016/06/Screen-Shot-2016-06-08-at-2.17.49-PM-300x166.png" alt="Screen Shot 2016-06-08 at 2.17.49 PM" width="527" height="292" />

<h2> Classes </h2>

I created a class for looking up the API's identifiers, one for gaining information on regions that have ski areas, one for getting ski maps and their information, and one to handle the command line interface, and I stored the index as a fixture, because all my classes use it and it is a large file to download on the fly. I learned about configuring, packaging, and releasing a project to RubyGems, and used the object oriented Ruby I know so far.

<img class="wp-image-116 aligncenter" src="http://www.benjaminlouisshapiro.com/wp-content/uploads/2016/06/Screen-Shot-2016-06-08-at-2.21.23-PM-300x146.png" alt="Screen Shot 2016-06-08 at 2.21.23 PM" width="537" height="261" />

<h2> Refactoring </h2>

Once I got the app working, I realized that it had CLI elements in classes that should just manage objects like Regions. I refactored so that the CLI was all in one place, the objects all had their own classes and so that the scrapers for the XML data from the API had their own classes. One other thing I need to change was that I had used require to load all my files from my environment file, and because of this, if I cd out of the main directory, they don't load properly, so I changed those lines to require_relatives.

<img class=" wp-image-130 aligncenter" src="http://www.benjaminlouisshapiro.com/wp-content/uploads/2016/06/Screen-Shot-2016-06-17-at-9.15.25-AM-300x135.png" alt="Screen Shot 2016-06-17 at 9.15.25 AM" width="482" height="217" />

If you like ski maps or you would like to see an app like this expanded and improved, feel free to install or fork the gem!
