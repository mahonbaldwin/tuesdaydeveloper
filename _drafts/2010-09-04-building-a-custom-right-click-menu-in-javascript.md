---
layout: post
title: "Building a Custom Right-click Menu in Javascript"
date: 2010-09-04 19:27:04.0
author: mahon
categories: 
---
I've been working on a web application project for the last couple weeks and I've decided that (since it's a web application) I should build a custom right click menu, also called a context menu. This resulted in a Google search with plenty of pie (results) but very little filling (substance). Most of the results I got were worthless. Many of them were demos and tutorials for flash, and since I wanted to use unadulterated DHTML and JavaScript they didn't help much. The results I did get where there was a working custom context menu where either poorly organized, way too long and bulky, or genuinely untrustworthy. So I decided to come up with my own.

The first step is disabling the default context menu (the one you would see if you were to right click somewhere on the page right now. It is pretty easy to disable the right click button in any web page, in fact it's only one line of code inside the <code>body</code> tag:
<code>
</code>
<pre lang="html" line="1"></pre>

&nbsp;

<code></code>Next we'll write the html page where we will be using our menu.
 <code>
</code>

<pre lang="html" line="1"></pre>
<div onmouseup="showMenu(event)">
        This is where your context will go.</div>
&nbsp;

&nbsp;

<code></code><strong>You must use <code>onmouseup</code> rather than <code>onclick</code> because the mouse events are not always captured using onclick. You will also want to pass the event as a parameter or some browsers will not support it.</strong>

Next we will add another div, the menu, that will be hidden. On the same page (after the other content before the closing <code>body</code> tag):<code>
</code>
<div id="menu"><a href="http://www.tuesdayDeveloper.com">tuesdayDeveloper</a>
<div onclick="doSomething()">Do Something</div>
&nbsp;

</div>
&nbsp;

<code></code>Inside the menu div you can put links, onEvent Links, etc.

Following is the JavaScript, the real fun:<code>
</code>
<pre lang="javascript" line="1"><script type="text/javascript">// <![CDATA[
function showMenu(event) {
     /*  check whether the event is a right click 
       *  because different browser (ahem IE) assign different numbers to the keys to
       *  your mouse buttons and different values to the event, you'll have to do some evaluation
       */
     var rightclick; //will be set to true or false
     if (event.button) {
          rightclick = (event.button == 2);
     } else if (e.button) {
          rightclick = (event.which == 3);
     }

     if(rightclick) { //if the secondary mouse botton was clicked
          var menu = document.getElementById('menu');
          menu.style.display = "block"; //show menu

          var x = e.clientX; //get X and Y coordinance for menu position
          var y = e.clientY;

          //This section is necessary if you click on the far right edge or bottom
          //The 200 is arbitrary, choose whatever number you want based on how large your menu is
          if(window.innerWidth) {
               windowWidth = window.innerWidth;
               windowHeight = window.innerHeight;
          } else if(document.documentElement.clientWidth) {
               windowWidth = document.documentElement.clientWidth;
               windowHeight = document.documentElement.clientHeight;
          } else {
               windowWidth = document.getElementsByTagName('body')[0].clientWidth;
               windowHeight = document.getElementsByTagName('body')[0].clientHeight;
          }
          if(windowWidth < (x + 200)) {
               x = x - 200;
          }
          if(windowHeight < (y + 200)) {
               y -= 200;
          }

          //position the menu
          menu.style.position = "fixed"; // use fixed or it will not work when the window is scrolled
          menu.style.top = y+"px";
          menu.style.left= x+"px";
     }
}

function clearMenu() { //used to make the menu disappear
     //this function should be used at the beginning of any function that is called from the menu
     var menu = document.getElementById('menu');
     menu.style.display = "none"; //don't show menu
}
// ]]></script></pre>
Following this tutorial you will get the barebonest of the barebones context menu. Obviously you can add style, functionality and whatever else you want. If you have any questions, feel free to ask.

<div class='archived comments'>

<div class='comment'>did you tested in Firefox?

works fine for me in IE, Safari and Chrome, but not in Firefox  <div class='by'>john on 2012-01-20 06:54:18.0  </div></div>
<div class='comment'>thank you for providing such an understandable and easy code  <div class='by'> on 2012-03-25 23:46:49.0  </div></div>
<div class='comment'>Thanks a lot, the best tutorial Ive found!  <div class='by'>Nic on 2012-06-16 11:17:04.0  </div></div>
<div class='comment'>Could you please put all of the parts into one code bit. I'm a bit confused where to put things! Thanks! ☺  <div class='by'> on 2013-06-01 01:55:20.0  </div></div>
<div class='comment'>hello,

i am sorry, but two out of the tree codes are not here.
Tried it in firefox, chrome and ie. But the code is still missing :(

greetings  <div class='by'>unknown on 2014-04-04 08:26:32.0  </div></div>
<div class='comment'>Two out of 3 codes are not here. I can't try a thing.

Can you solve that, please?

Thanks  <div class='by'>Anonymus on 2015-03-14 22:50:58.0  </div></div>
<div class='comment'>You're missing part of your code...  <div class='by'> on 2015-04-15 19:45:30.0  </div></div>
<div class='comment'>fvf  <div class='by'>thstr on 2015-09-28 02:11:18.0  </div></div>
</div>