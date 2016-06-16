---
layout: post
title: "Android ORM"
date: 2012-09-04 14:08:50.0
author: mahon
categories: 
---
Personally, I don't know why ORM isn't the default in Android. I'm sure Google could figure it out. But I'm not going to get into that since there is a way to use this feature in your programming.

<a title="ORMLite" href="http://ormlite.com/">ORMLite</a> is a library that creates and helps you retrieve the right information in your Android projects. Using it isn't difficult, but you will need to set up your Java Objects and your Data Access Objects (DAO).

To get started simply download the latest libraries from <a href="http://sourceforge.net/projects/ormlite/files/releases/com/j256/ormlite/">SourceForge</a>, you'll need the ormlite-core and ormlite-android jar files. Put both of these jar files into the libs directory of the project you are working on.

Next you'll need to create an object that uses the annotations