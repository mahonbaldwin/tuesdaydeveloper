---
layout: post
title: "Lazy vs. Eager Fetching in Java EE"
date: 2009-12-09 11:29:00.0
author: mahon
categories: 
---
So you have a relational database and you want to see what <code>Addresse</code>s, <code>Phone</code> numbers, or <code>Account</code>s are associated with a <code>Person</code>. By default, Java EE will only get the objects it absolutely needs; this is called lazy fetching. So solve this problem you want to fetch eagerly. In your code, find the attribute in your entity bean that you want to eagerly fetch and edit the attributes to indicate that it should eagerly fetch the information:
<div>In the <code>Person</code> bean:</div>
<div class="codeblock"><code> @OneToMany(mappedBy="person", fetch=FetchType.EAGER)
 private Set accounts;</code></div>
<div>Obviously there may be some changes you need to make, <code>@OneToOne</code>, <code>@ManyToOne</code>, <code>@ManyToMany</code>, etc. It will vary based on the circumstance.</div>

<div class='archived comments'>

<div class='comment'>You are spreading the wrong idea here. @Basic fields, all those primitive types for example, defaults to EAGER loading. So does relationships with a cardinality of "one-to-one" and "many-to-one". Therefore, it is only @OneToMany and @ManyToMany fields that the developer might have the need to explicitly set to use EAGER loading. Also note that FetchType.LAZY is just a hint to the provider.  <div class='by'>Martin Andersson on 2014-07-03 18:15:49.0  </div></div>
</div>