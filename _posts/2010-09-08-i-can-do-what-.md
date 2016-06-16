---
layout: post
title: "I can do WHAT??? "
date: 2010-09-08 00:16:16.0
author: mahon
categories: 
---
Tonight I learned a life-saving technique that I will forever hold dear to my heart. While reading <a href="http://www.webcredible.co.uk/user-friendly-resources/css/css-tricks.shtml">Ten CSS tricks you may not know</a>, I learned a CSS trick that I, indeed, did not know. Did you know that you can assign multiple classes to one element? Yes, you can, here's how:
<pre lang="html" line="1">&lt;style type="text/css"&gt;
     .red {
          color:red;
     }

     .bold {
          font-weight:bold;
     }
&lt;/style&gt;
&lt;p class="red"&gt;This is a red div.</p&gt;
&lt;p class="bold"vThis is a bold div.</p&gt;
&lt;p class="red bold"&gt;This is both red and bold!</p&gt;
</pre>

If you try it out yourself, you'll notice that there are only to styles defined and that both styles are attached to the final paragraph. All you have to do is delimit the class names with a space. Thank you <a href="http://www.webcredible.co.uk/">Webcredible</a>!

<div class='archived comments'>

<div class='comment'>You can also style on a custom attribute.  So, if you have an element like this:
...
You can style this li like this:
li[tab="account"] { border: 1px solid pink; }

If you have multiple elements with the same custom attribute, only with different values, you can style them all like this:
li[tab] {border 1px solid red; }

I use custom attributes all the time for my javascript, because I know nobody will mess with them.  This helps us keep the html required for the javascript separate from the html needed for the styling.  But you can use them for both.  <div class='by'>Devin Baldwin on 2010-10-04 00:00:41.0  </div></div>
</div>