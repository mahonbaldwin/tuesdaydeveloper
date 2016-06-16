---
layout: post
title: "jQuery Selector Test"
date: 2011-07-20 09:16:01.0
author: mahon
categories: 
---
The other day I was talking to one of my coworkers who asked me an interesting question. He wanted to know if there was any significant improvement, when using jQuery, to assigning a jQuery selector to a variable when you repeatedly are referencing the same object. Is there much overhead when jQuery instantiates the variable?

Neither he nor I doubted that there was some overhead, but we both wanted to have a more clear picture so I devised the following test:

<pre lang="javascript" line="1">&lt;script type="text/javascript"&gt;
	var tests = 100000;
	$(document).ready(function(){
		var date1a = new Date();
		edit1();
		var date1b = new Date();
		var date2a = new Date();
		edit2();
		var date2b = new Date();
		$('#one').html(date1b-date1a + " ms");
		$('#two').html(date2b-date2a + " ms");
	});
	function edit1(){
		for(var i = 0; i < tests; i++){
			$('#one').html(i);
		}
	}
	function edit2(){
		var two = $('#two');
		for(var i = 0; i < tests; i++){
			two.html(i);
		}
	}
&lt;/script&gt;</pre>
And:

<pre lang="html" line="1">&lt;div id="main"&gt;This is a test page.&lt;/div&gt;
&lt;div id="one"&gt;Test 1&lt;/div&gt;
&lt;div id="two"&gt;Test 2&lt;/div&gt;</pre>

The results were pretty consistent as follows:
<pre lang="">
     This is a test page.
     3007 ms
     2497 ms
</pre>