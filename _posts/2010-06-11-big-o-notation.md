---
layout: post
title: "Big O Notation"
date: 2010-06-11 20:17:15.0
author: mahon
categories: 
---
Trying to understand how an algorithm works? Are people trying to tell you how effective it is? They're probably using Big O Notation.

Big O Notation (also known as Big Oh notation, Landau notation, Bachmann–Landau notation, and asymptotic notation) is used to help developers and engineers measure the speed or memory an algorithm will take. It generally measures the best, worst, and/or average case scenario. This is a short guide, for more information look at <a href="http://rob-bell.net/2009/06/a-beginners-guide-to-big-o-notation/">this recourse</a> where I got a lot of this information.

Big O Notation always contains a formula inside of parenthesis that indicate the information you will need to know. You will often see an n in the formula, this n is indicative of the number of elements in an array. You may also see logarithms, exponents, or other mathematical values or computations. Here is a list of some common O notations and what they mean.

<strong>O(1)</strong>
O(1), O(101), or any other number describes an algorithm that will always finish in the same time. It will take exactly as long or as many recourses to make its calculation no matter how long or short an array of data is. If you were to view this visually, you would see a flat line on a graph.

<a href="/uploads/2010/06/Screen-shot-2010-06-11-at-5.35.12-PM.png"><img class="alignleft size-full wp-image-360" title="O(n)" src="/uploads/2010/06/Screen-shot-2010-06-11-at-5.35.12-PM.png" alt="O(n)" width="207" height="233" /></a><strong>O(n)</strong>
O(n) describes a linear relationship between the number of data elements and the amount of time or memory an algorithm will take. In other words, an array with 100 elements will take 10 times as long as an array with just 10 elements. Visually you would see a straight line increasing upward at the same rate that is it progressing forward.

<strong>O(n<sup>2</sup>)</strong>
O(n<sup>2</sup>) describes an exponential relationship between the number of elements in an array and the amount of time it will take to complete. On a graph this would be exponential line where the line starts at a slight incline and progresses to a near vertical slop. (We like to avoid these kind of algorithms if at all possible, since they are memory hogs.)

<strong>O(n log n)</strong><a href="/uploads/2010/06/n-log-n.png"><img class="alignright size-full wp-image-361" title="O(n log n)" src="/uploads/2010/06/n-log-n.png" alt="O(n log n)" width="282" height="236" /></a>
O(n log n) indicates that an algorithm will take the number of elements times the logarithm of the number of elements. If you were to look at a graph of this, it wouldn't be linear and it would be exponential, it would be somewhere in between. You would see a slop that started out climbing at a quick pace and then slowed it's progress to appear more linear.

<strong>Conclusion</strong>
Obviously, there are many permutations of this practice (as there are an infinite number of ways to write an equation) but you get the idea. Feel free to ask any questions? I hope I'll have the answer. ;)
<div id="clear"></div>

<div class='archived comments'>

<div class='comment'>Great post! But maybe if you add examples to each case, e.g. O(n log n) is for merge sort, etc. Thanks!  <div class='by'>bobo on 2013-09-08 16:55:02.0  </div></div>
</div>