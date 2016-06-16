---
layout: post
title: "Eclipse Hint: Clean Your Projects"
date: 2009-11-16 20:23:00.0
author: mahon
categories: 
---
Earlier today when I was working on a project I took a look at one of my entity beans and it had all sorts of errors in it. Seeing as how I had <a href="http://tuesdaydeveloper.com/?p=7">Eclipse create these for me</a> and I didn't edit the code or the database at all, I was a bit confused. When I looked at the project file listings I noticed that Eclipse said all of the beans had compile errors in them.

Naturally I restarted Eclipse to see if that would help, nope. I then went to Project > Clean and selected my EJB project, sure enough, all the errors when away.

Eclipse is a <i>wonderful</i> tool, but even the best tools have their moments.