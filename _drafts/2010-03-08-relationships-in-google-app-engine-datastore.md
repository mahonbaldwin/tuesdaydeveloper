---
layout: post
title: "Relationships in Google App Engine Datastore"
date: 2010-03-08 16:05:11.0
author: mahon
categories: 
---
Today I needed to add a reference to an <code>Object</code> from another <code>Object</code> stored inside Google App Engine's Datastore. Now, this is incredibly simple—there was only one problem—I was creating both objects at the same time. For one-to-one relationships, you can use an attribute of one of your beans to store the data (e.g. <code>private Address address;</code> inside the <code>Person</code> bean).