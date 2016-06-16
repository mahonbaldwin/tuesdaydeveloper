---
layout: post
title: "Making SQLite Work"
date: 2011-11-01 21:52:13.0
author: mahon
categories: 
---
So I've been playing around with creating an offline application. Because I want dynamic data, I needed to figure out how to use a SQLite database. Honestly, I feel quite foolish because I was sure that I was doing everything correctly, following the <a href="http://www.bennadel.com/blog/1940-My-Safari-Browser-SQLite-Database-Hello-World-Example.htm">tutorial</a> (nearly) perfectly (sans "Girls" application, I enjoy being married!).

Despite working at if for several hours nothing I was doing was working. Then I caught it—the typo. Inside of my insert statement I had miss-typed the name of one of the table fields.

No errors. No warnings. No worky.

I guess this is my lesson that with a SQLite database there is no forgiveness. You do it right or you don't do it. Apparently I just needed a reminder that I'm human.