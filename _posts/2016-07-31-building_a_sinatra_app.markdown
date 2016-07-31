---
layout: post
title:  "Linkfiend, Social Bookmarking with Sinatra"
date:   2016-07-30 20:18:31 -0400
---

I decided to build a social bookmarking app using Sinatra because I thought it was something I would like to have. There are many options for social bookmarking. The best one in my opinion, Pinboard, costs $11/year. I've used it for many years and I wanted to see if I could build a similar app with Sinatra, and I felt that I could put what I had learned about CRUD, RESTful routes, and MVC architecture to the test in this domain. 

To build the app, I used the bcrypt gem to encrypt user passwords, sqlite and ActiveRecord for the database, and Sinatra, Rack and ERB templating to build the core app. Before starting, I mapped out what associations my models would need. Bookmarks, Users, Tags, and Lists would have the following associations.

> 1. Bookmark
> Has a link
> Has a description
> Has many tags
> Has many Tags
> Has a private/public designation
> 2. User
> Has a username
> Has a password
> Has many bookmarks
> Has many tags
> 3. Tag
> Has many bookmarks
> Has a user
> 4. List
> Has a user
> Has many bookmarks

I used slugs as the method of identifying objects such as users and tags, creating a slugafiable module with a class method for finding objects and an instance method for taking a name and slugifying it. I also enjoyed making it pretty using Bootstrap. Generally, the site looks like this:

[Imgur](http://i.imgur.com/SmBFLCE.png)

I decided after I had build most of the functionality that if someone just gives a URL, that the app should scrape the title and attempt to scrape a description from the metadata and automatically put it into a new bookmark.

One of the things I learned from this project was that nil values need to be handled carefully because they can easily make Sinatra throw an error. I also see the need for an easier way to only let logged in users see content, as this could potentially become unmanageable for a larger project. 

Though I didn't get to implement them on this app, I'm interested to see some of the things form objects, decorators, and view models can do in a rails app. Some things I still need to do is encapsulate some of the CSS into a separate style sheet and maybe add to the list feature.


