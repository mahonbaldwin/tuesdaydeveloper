---
layout: post
title: "Rounding Errors"
date: 2016-02-09 12:34:43.0
author: mahon
categories: 
---
I'm not really sure why, but since I learned about rounding errors I've always been fascinated by them. Case in point, I was playing with Clojure's (range) function and:

<pre lang="clojure">(range 0.1 1.1 0.1)
=>(0.1 0.2 0.30000000000000004 0.4 0.5 0.6 0.7 0.7999999999999999 0.8999999999999999 0.9999999999999999 1.0999999999999999</pre>

When I tried this out I was expecting ten results&mdash;not eleven&mdash;something more like:

<pre lang="clojure">(map #(/ % 10.0) (range 1 11))
=>(0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1.0)</pre>