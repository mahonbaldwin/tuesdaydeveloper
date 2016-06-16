---
layout: post
title: "Comparing Two Collections"
date: 2009-12-08 19:31:00.0
author: mahon
categories: 
---
This is a valuable piece of code that I am sure I'll be using in the future:
<pre>public boolean isSame(Collection expected, Collection actual) {
 /* first compare the sizes, if they are not the same there is no use in going further */
 if(expected.size() != actual.size()) {
  return false;
 }
 /* the count variable will increment for each match */
 int count = 0;
 /* The iterators check each object in each Collection */
 for (Iterator iterator = expected.iterator(); iterator.hasNext();) {
  Object object = (Object) iterator.next();
  for (Iterator iterator2 = actual.iterator(); iterator2.hasNext();) {
   Object object2 = (Object) iterator2.next();
   if(object2.equals(object)) {
    count++;
   }
  }
 }

 /* the count variable is the same as the size of the Collections then all items match */
 if (count != expected.size()) {
  return false;
 }
 else {
  return true;
 }
}
</pre>
<div>I wrote it to check if an expected <code>Set</code> was the same as an actual <code>Set</code>. I chose to use a <code>Collection</code> to make it more generic, and since a <code>Set</code> is a <code>Collection</code> it will still work.</div>