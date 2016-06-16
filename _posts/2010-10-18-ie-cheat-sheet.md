---
layout: post
title: "IE Cheat Sheet"
date: 2010-10-18 23:13:44.0
author: mahon
categories: 
---
Dealing with browsers is like dealing with children (Disclaimer, I am not giving parental advice). Most parents I know with more than one child try to treat each of their children fairly, but that doesn’t always mean that each child is treated the same. For a teenager who is always bending the rules, they may set a curfew of ten o’clock; but for the children that are obedient, who usually try to follow the rules, parents may let them stay out until midnight.

Internet Explorer is like a bad child. The rules you set for this browser by necessity must be different than the others. And for good reason, IE doesn’t play nicely with others, it continually disobeys rules, and when it does follow rules, it follows them the way it wants to (not the way you told it to). Following is a list of <em>rules</em> that I’ve found helpful when dealing with IE. The rules will be mostly javascript, but there will be some CSS fixes. As I find other helpful tips I’ll add them to this post. If you know of a rule that is helpful in dealing with IE, please share (I’ll update my post to include your suggestions).

Please know, that while these tips usually work in IE, they sometimes don't, hey children are unpredictable. On this post, these have been tested in IE 8.

<br /><br /><strong>Event handling</strong>
The event in IE is global. This means that you must call events differently than in other browsers. In many ways this is a nice thing, you can call an event without passing it as a parameter into a function. You could set a global variable in your javascript so that other browsers would function the same way, but I like to limit the global variables I use so I do the following in my functions.
<pre lang="javascript" line="1">function someFunction(event){
     event = event || window.event;
     //...
}</pre>This snippet of code looks to see if the event was null when passed in, if the variable is empty the code will assume that the browser is IE and set event to the window.event global variable. You can now go on using the <code>event</code> variable without having to worry to change the way your function works in different browsers.


<br /><br /><strong>Mouse X and Y Coordinates</strong>
Microsoft Internet Explorer doesn't support <code>clientX</code> and <code>clientY</code>, in stead, <code><em>pageX</em></code> and <code><em>pageY</em></code> are used. Use the following to get the correct X or Y coordinates for either IE or another browser.
<pre lang="javascript" line="1">function someFunction(event){
     event = event || window.event;
     var x = (event.pageX) ? event.pageX : event.clientX;
     var y = (event.pageY) ? event.pageY : event.clientY;
}</pre>

If ternary are confusing to you, use:
<pre lang="javascript" line="1">function someFunction(event){
     event = event || window.event;
     var x;
     var y;
     if (event.pageX) {
          x = event.pageX;
          y = event.pageY
     } else {
          x = event.clientX;
          y = event.clientY;
     }
}</pre>If you are not looking for coordinates of a mouse event, but rather for an object, this is the script that I've found most helpful:
<pre lang="javascript" line="1">function findPos(el) {
	var y = 0;
	var x = 0;
	while(el != null) {
		y += el.offsetTop;
		x += el.offsetLeft;
		el = el.offsetParent;
	}
	return [x, y];
}</pre>
This function returns an array, so when you call it (<code>var position = findPos(el);</code>) you will need to get the x value as <code>position[0]</code> and the y value as <code>position[1]</code>.


<br /><br /><strong>Fixed Positioning</strong>
It's really quite painful that IE doesn't support the fixed position. Sometimes it does fine, but other times you div is all messed up. This following script will help fix fixed. All of the examples so far have used normal javaScript, in truth I prefer to do things with <a href="http://jquery.com/">jQuery</a>, but only when its more efficient (usually it is). This following script is just easier if you do it with jQuery (be sure to include the <a href="http://ajax.googleapis.com/ajax/libs/jquery/1.4.3/jquery.min.js">jQuery library</a>:
<pre lang="javascript" line="1">$(window).scroll(function() {
    $('#myElement').css('top', $(this).scrollTop() + "px");
});</pre>

If you prefer to not use the jQuery library, <a href="http://www.daniweb.com/code/snippet217425.html">this reference</a> seems to work.

<br /><br /><strong>Source Element</strong>
When you click an element, the DOM can tell you which element you clicked, but, of course, you have to work around to get it to work in IE.
<pre lang="javascript" line="1">
     //..
     event = event || window.event;
     var sourceElement = event.target || event.srcElement;
     //..</pre>

<div class='archived comments'>

<div class='comment'>I know it still has a lot of users, but I just don't bother developing for IE anymore. I figure if they can't be bothered to write a good browser, I can't be bothered to write websites for it. If enough coders stopped making their sites work in IE, I think a bigger chunk of the user base would eventually "get it" and move on to Firefox.  <div class='by'>Mark Richardson on 2010-10-19 09:51:31.0  </div></div>
<div class='comment'>Yeah, I've thought along those same lines before. At the moment, however, I don't really have a choice, since my employer has asked me to develop for all browsers. If I could, I would develop for "modern browsers"—I don't consider IE to be "modern". I mean, in the new IE 9 I tested the :hover pseudo selector and it didn't work!

So just out of curiosity, do you notify IE users that your page is ment to be viewed in another browser? How many visiters do you have that use IE?  <div class='by'>Mahon on 2010-10-20 08:59:43.0  </div></div>
<div class='comment'>I don't notify although I've thought of doing that. Majority of my traffic seems to be firefox, with IE in second place. If there is something to do with with functionality that is broken on IE, I might fix it... otherwise I don't bother. If my site looks different or weird in IE, I think of it as punishment for the user using a non-modern browser. Probably not the best, but it works for me.  <div class='by'>Mark Richardson on 2010-10-20 09:55:23.0  </div></div>
</div>