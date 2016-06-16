---
layout: post
title: "Magic Quotes with PHP"
date: 2010-04-27 16:19:56.0
author: devin
categories: 
---
I have two development Environments: <a href="http://www.mamp.info">MAMP</a> (Macintosh, Apache, MySQL, PHP) and IIS with PHP.

I created a development tool in PHP a that simply decodes a JSON string and outputs it for easy viewing:
<pre>&lt;form method="post" style="padding-bottom:20px;"&gt;
&lt;textarea name="json" cols="75" rows="10"&gt;&lt;/textarea&gt;&lt;br /&gt;
&lt;input type="submit" /&gt;
&lt;/form&gt;

&lt;?php
if(!empty($_POST['json']))
{
	echo '&lt;pre&gt;';
	print_r(json_decode($_POST['json']));
	echo '&lt;/pre&gt;';
}
?&gt;</pre>
The tool worked well in the IIS with PHP environment, but would choke out every time in MAMP. It kept automatically escaping single quotation marks ('), double quotation marks (") and back-slashes (\).

It was a php.ini thing, not an environmental thing. The fix? Open up the php.ini file, and make sure your magic_quotes_gpc variable is turned Off, like this:
<pre>magic_quotes_gpc = Off</pre>
I believe this is default for most PHP environments, except MAMP.