---
layout: post
title:  "A Rails App to Rate Quality of Life Over Time"
date:   2016-12-07 22:55:30 +0000
---


I have been wanting to make this app for a couple of months now, but I have been extremely busy until now. My main motivation to create this app was to contribute to the open source mental health app called if-me.org, an app created by Julia Nguyen, and which I will try to integrate into their rails app once I have this quality of life tool polished and perhaps add visualization and graphs for the different factors in quality of life that are measured and the category and total ratings that users receive over time. I may use D3 for this visualization when I learn enough Javascript. The app is thus a work in progress, and something I will try to refactor and improve over the next few weeks. 

This was the first Rails app I have made without the guidance of a tutorial, and I have used the concepts in Rails that I have learned throughout the Rails unit at learn.co. It seems like a long time since my last assessment, and I'm looking forward to this one and finishing up the curriculum over my break from school. 

To build the app, I used a couple of outside gems to accomplish my app, such as deep-clone, omniauth, and bcrypt. I tried to stay close to the conventions of REST APIs but I found that my needs had me deviating from those conventions. I would like to refactor by encapsulating some of the logic that is in my controllers into my models, since I know that is a better design practice, although sometimes it seems to add unnecessary complexity and passing around of variables to take bis logic from controllers to models.

Making the questionnaire functional required a bit of quasi nested form code. I'm sure it would have been easier and more elegant with Javascript and perhaps I will come back and do some further refactoring with Javascript when I am able.
