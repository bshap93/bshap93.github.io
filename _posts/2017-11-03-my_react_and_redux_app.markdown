---
layout: post
title:      "My React and Redux App"
date:       2017-11-03 23:47:13 +0000
permalink:  my_react_and_redux_app
---


The final self-inspired app in the curriculum at Learn.co seemed at least as simple as the other projects, and ultimately felt the most expressive and powerful. I made a TV show tracker, where the user can see popular shows, search shows, and add them to their My Shows list and from there be able to track the show. There were some things I was asked to do such as getting the React client to work with the Rails API that I got hung up on. One piece of functionality I have yet to implement but will very soon is the Rails backend being able to refresh its Show objects in order to detect if there has been a change in the number of aired episodes and that involves hooking up the Rails backend to the trakt.tv API using Faraday. 

Apart from when I was stuck, I enjoyed making the decision on design and logic as I have on previous portfolio projects more than any other part of the Learn.co curriculum. Making something from first brainstormed idea to working prototype app is a rewarding process. 

I built the React and Redux parts of the app first and at the same time. I felt it would be harder if I built React components for my app and then add Redux. The Redux store was the main data-store in my Client side app that was consistent across the app. I used it most times unless I just wanted to use React State on its own for some small piece of app state. The Rails API is pretty minimal right now and just handles  the users shows that they have chosen for the My Shows list. I used the Fetch API built into Javascript to communicate between trakt.tv and my Client side app as well as between the Rails and Client side, as I was required. Using JQuery would have made things very confusing since React uses a virtual DOM while JQuery is concerned with the non-virtual DOM, and that is the reason I believe I was asked not to use it. 

Besides React and Redux related packages, and ES6, I did not use too many Javascript packages. I used dotenv, but no other packages. The app itself is quite simple in that sense. I used Rails New for the backend and Create-React-App to start my Client-side app. I used actions to trigger reducers to manage the data store. I had some services that accomplished fetching to or from the trakt API or the Rails API. This was for me a personal proof of concept in making React and Redux apps. I have lots of ideas for future apps using these technologies and look forward to honing my skill in React, Javascript and the backend of my choice.
