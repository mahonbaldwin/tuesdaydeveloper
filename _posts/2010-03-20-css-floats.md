---
layout: post
title: "CSS floats"
date: 2010-03-20 08:35:02.0
author: mahon
categories: 
---
I must admit that I was extremely confused when I was positioning <code>div</code>s with css today. I had two <code>div</code>s inside of another <code>div</code>. Something like the following.
<pre lang="HTML" line="1">
&lt;div id="container"&gt;
    &lt;div id="blue"&gt;This is the blue div.&lt;/div&gt;
    &lt;div id="red"&gt;This is the red div.&lt;/div&gt;
&lt;/div&gt;</pre>
The css for the page was as follows:
<pre lang="CSS" line="1">div#container {
    width:80%;
    margin-left:auto;
    margin-right:auto;
    border:solid #000000;
    border-width:5px;
}

div#blue {
    background-color:blue;
    width:50%;
    float:left;
}

div#red {
    background-color:red;
    width:50%;
    float:left;
}</pre>
What I expected and what I got were two different things.

Expected:
<a href="/uploads/2010/03/Screen-shot-2010-03-20-at-9.58.01-AM.png"><img class="aligncenter size-full wp-image-145" title="Screen shot 2010-03-20 at 9.58.01 AM" src="/uploads/2010/03/Screen-shot-2010-03-20-at-9.58.01-AM.png" alt="" width="580" height="82" /></a>

Actual:
<a href="/uploads/2010/03/Screen-shot-2010-03-20-at-9.58.22-AM.png"><img class="aligncenter size-full wp-image-146" title="Screen shot 2010-03-20 at 9.58.22 AM" src="/uploads/2010/03/Screen-shot-2010-03-20-at-9.58.22-AM.png" alt="" width="565" height="51" /></a>

Why did this happen? Well, apparently if you use the float option in css, it doesn't mean that the <code>div</code>s will still look like they are in the container. The easiest way to fix this is with a <code>&amp;nbsp;</code> or a <code>&lt;br /&gt;</code> somewhere after the second <code>div</code>, though that also has its consequences. Probably the best way i've found is to use a div with a clear attribute:
<pre lang="HTML" line="1">
&lt;div id="container"&gt;
    &lt;div id="blue"&gt;This is the blue div.&lt;/div&gt;
    &lt;div id="red"&gt;This is the red div.&lt;/div&gt;
&lt;/div></pre>
With the css for the div:
<pre lang="CSS" line="21">#clear {
    clear:both;
}</pre>
That works quite nicely!