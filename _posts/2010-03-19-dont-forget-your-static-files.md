---
layout: post
title: "Don't forget your static files"
date: 2010-03-19 20:44:15.0
author: mahon
categories: 
---
Before you deploy your app in Google App Engine, you must configure your app to include static files. Important files—like your css files! So, if you want to have your application look the same in testing and in deployment, then follow these steps.

<strong>Open appengine-web.xml</strong>
The first thing you'll need to do is fine the appengine-web.xml file located in <em>[your program folder]</em> &gt; war &gt; WEB-INF. It should look something like this (in Source view):

<pre lang="XML" line="1">
&lt;appengine-web-app xmlns="http://appengine.google.com/ns/1.0"&gt;
     &lt;application&gt;application&lt;/application&gt;
     &lt;version&gt;1&lt;/version&gt;

     &lt;!-- Configure java.util.logging -->
     &lt;system-properties&gt;
          &lt;property name="java.util.logging.config.file" value="WEB-INF/logging.properties"/&gt;
     &lt;/system-properties&gt;
     &lt;ssl-enabled&gt;true</ssl-enabled&gt;
     &lt;sessions-enabled&gt;true</sessions-enabled&gt;
&lt;/appengine-web-app&gt;</pre>

Once it's open, you'll need to insert a <code>static-files</code> tag into the file (it doesn't matter where, but I usually put it after the <code>version</code> attribute). Between the open and close <code>static-files</code> tags put the <code>include</code> tag with a<code>path</code> attribute. It should look like this:

<pre lang="xml" line="5">
&lt;static-files&gt;
      &lt;include path="/favicon.ico" /&gt;
      &lt;include path="/css/screen.css" /&gt;
      &lt;include path="/img/*.png" /&gt;
      &lt;include path="/js/*" /&gt;
&lt;/static-files&gt;</pre>

As you can see, you can use the * wildcard so in order to save a lot of potential typing.

All together it should look like this:

<pre lang="xml" line="1">
&lt;?xml version="1.0" encoding="utf-8"?>
&lt;appengine-web-app xmlns="http://appengine.google.com/ns/1.0"&gt;
     &lt;application&gt;prs-app</application&gt;
     &lt;version&gt;1&lt;/version&gt;
     &lt;static-files&gt;
	&lt;include path="/favicon.ico" /&gt;
  	&lt;include path="/css/screen.css" /&gt;
  	&lt;include path="/img/*.png" /&gt;
  	&lt;include path="/js/*" /&gt;
     &lt;/static-files&gt;

     &lt;!-- Configure java.util.logging --&gt;
     &lt;system-properties&gt;
         &lt;property name="java.util.logging.config.file" value="WEB-INF/logging.properties"/&gt;
     &lt;/system-properties&gt;
     &lt;ssl-enabled&gt;true&lt;/ssl-enabled&gt;
     &lt;sessions-enabled&gt;true&lt;/sessions-enabled&gt;
&lt;/appengine-web-app&gt;</pre>