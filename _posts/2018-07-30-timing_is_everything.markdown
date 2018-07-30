---
layout: post
title:      "Timing is everything"
date:       2018-07-30 07:47:12 -0400
permalink:  timing_is_everything
---


Today I finished my first Java Script project. I can honestly say it was the most challenging one yet and one of the most satisfying to complete.

The assignment was to take my previous rails project and add dynamic features that are possible only through jQuery and a JSON API. Sounds easy enough, right?

Well, it wasn’t… the implementation of JS into my app proved to be more challenging than I expected (and more rewarding when completed. There were a few bumps in the road (coding is rarely smooth sailing…). I would like to point out one of those and maybe it would help others who are new to JS and to working with API’s

A good analogy for adding AJAX calls to your app is sending your kid to his first day of school. Up until then you were the one communicating with it and it was all done in a local environment. Now you send it into the world and your app needs to learn how to communicate with his teacher and the other pupils. 

When I implemented the get calls through Jqueary I ran into a problem. The data I was getting was what I wanted but my app was displaying something else, sometimes the data was missing some information and sometimes it just did not display at all. I was baffled. I didn’t know why this happened so I started my detective work. First I made sure that my developer console logs the XMLHttp request (you can do so by checking the box in the console settings). This is important because it shows you the order of events that are taking place when running your code. 

When I examined what was happening I noticed that sometimes the function that was sending the request would finish running before the response to it was received. My function included both the get request and the logic of what to do with the response. Once I realized that this is probably the source of the problem I started looking for a solution. 

AJAX stands for *“Asynchronous JavaScript And XML”*. That means that at it’s core it is not synced. There is no action -> reaction -> another action flow. My function was running and not waiting for all the data to come… I had to find a way to tell my function to hold and wait till the AJAX request was done so all the data is available.

The solution was easy. AJAX has a .done function that basically tells the function to wait till the AJAX process was finished before executing the callback function. Once I added that to my code, everything started running smoothly.

