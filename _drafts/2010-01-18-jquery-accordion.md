---
layout: post
title: "jQuery Accordion"
date: 2010-01-18 15:50:14.0
author: mahon
categories: 
---
I've been searching for a good tutorial to build an accordion menu using jQuery. I've been looking for about an hour now and I still haven't found any simple (step-by-step) instructions. So I decided to make my own.

An accordion-style menu has multiple selections available that open and close, only one option may be open at any time. If you ope one, it will close another if it is open.

Step 1: Set up your html.
Depending on your needs, you'll want different types of hypertext markup. You may want divs, lists, etc., for this tutorial I'll be using an unordered list:
<pre><code>&lt;html&gt;
&lt;body&gt;
   &lt;ul&gt;
      &lt;li&gt;Things
         &lt;ul&gt;
            &lt;li&gt;Thing 1&lt;/li&gt;
            &lt;li&gt;Thing 2&lt;/li&gt;
            &lt;li&gt;Thing 3&lt;/li&gt;
            &lt;li&gt;Thing 4&lt;/li&gt;
         &lt;/ul&gt;
      &lt;/li&gt;
      &lt;li&gt;Stuff         &lt;ul&gt;
            &lt;li&gt;Stuff 1&lt;/li&gt;
            &lt;li&gt;Stuff 2&lt;/li&gt;
            &lt;li&gt;Stuff 3&lt;/li&gt;
         &lt;/ul&gt;
      &lt;/li&gt;
      &lt;li&gt;More Stuff
         &lt;ul&gt;
            &lt;li&gt;More Stuff 1&lt;/li&gt;
            &lt;li&gt;More Stuff 2&lt;/li&gt;
         &lt;/ul&gt;
   &lt;/ul&gt;
&lt;/body&gt;
&lt;/html&gt;</code></pre>