---
layout: post
title: "Co-custody + Google Datastore = Big No No"
date: 2010-03-12 23:46:56.0
author: mahon
categories: 
---
I learned something today while I was debugging some code: It turns out that a child class can only belong to one parent class in the datastore. If you attempt to use a child class in two different places, it may seem to work at first, but then you'll get an excetpion and error message like this:
<code>java.lang.ClassCastException: oid is not instanceof javax.jdo.identity.ObjectIdentity ...</code>
It was strange, one minute this code was working fine, after I worked on some other code I went back and it stopped working. Here is what I was trying to do:

I'm working on a project that will track real estate listings (<code>Listing</code>) and clients (<code>Person</code>). Each of those <code>Object</code>s need an <code>Address</code>, so I thought I'd just use the same bean like I would in with a relational database table. Turns out, however, you can't do that in Google's Datastore.

To quote Max Ross of Google, "Due to the nature of primary keys in the app engine datastore, a [sub]class can only have a single owner.  We detect other flavors of this scenario when the entity meta-data is loaded but not this particular variant" (<a href="http://www.mail-archive.com/google-appengine-java@googlegroups.com/msg04372.html">1</a>). Max did say later in the post that he would look into how they could repair that, but I don't really know if that's going to happen any time soon.

In order to not have this bug, I'll have to use two different <code>Address</code>es for those beans now. Sigh.