---
layout: post
title: "Query Without Primary Key in Java EE"
date: 2009-12-09 11:25:00.0
author: mahon
categories: 
---
It is extremely simple to get an entity bean object from a database based on the primary key:

<pre>public Person getPerson(String personId) {
 Person aPerson = manager.find(Person.class, personId);
 return aPerson;
}</pre>

<div><br />But what if you don't have the primary key, like when you want to log in and you only have the userid and the password?</div>
<div>For this you will need to actually write a query, but do not fear, it is very simple:</div>

<pre>public Person getPerson(String userid, String password) {
  if(userid == null || password == null) {
   return null;
  }
  try {
   Query q = manager.createQuery("select p from Person p where p.userid = :uid and p.password = :pass");
   q.setParameter("uid", userid);
   q.setParameter("pass", password);

   Person aPerson = (Person) q.getSingleResult();
    if(aPerson == null) {
     return null;
    }
    return aPerson;
   }
  catch (Exception e) {
   return null;
  }
}</pre>
