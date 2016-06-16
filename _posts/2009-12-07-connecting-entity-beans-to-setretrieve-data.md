---
layout: post
title: "Connecting Entity Beans to Set/Retrieve Data"
date: 2009-12-07 18:45:00.0
author: mahon
categories: 
---
<div>I've spent the last week figuring this out, ultimately I had one line that was wrong.</div>

<div>When you have the database set up, and the <a href="http://tuesdaydeveloper.com/?p=7">entity beans in an EJB project</a>, how do you connect to the server to access or update data?</div>

<div>Prerequisites: You should have <a href="http://tuesdaydeveloper.com/?p=7">entity beans in an EJB Project that reference a database</a>.</div>

<div><div><ol><li>You should have two classes set up in your EJB project: BusinessRules.java, BusinessRulesRemote.java (or whatever you chose to call them). These files have a special method to construction, follow the list below.<br /><ol><li>Secondary click the EJB project and select New > Session Bean</li><li>Enter the package and class name (I chose "session" and  "BusinessRules")</li><li>If you want to use a remote interface (best for scalability) you will need to select it. You may leave the local interface (best for performance) button checked or you may uncheck it, I unchecked it.</li><li>Click finish.</li></ol></li><li>Edit your BusinessRules.java and BusinessRulesRemote.java class.</li><ol><li>Now you need to implement your business logic.

<pre>@Stateless /*These business rules are stateless.*/
public class BusinessRules implements BusinessRulesRemote { /*class header*/
@PersistenceContext /*If you are using Java EE then you will need this to get the Persistent context*/
EntityManager manager; /*The manager you'll be using*/

public static final String REMOTEJNDINAME = BusinessRules.class.getSimpleName() + "/remote"; /*JNDI name so that the class can be found*/

public BusinessRules() { //Default constructor
}</pre>

</li></ol></ol></div><div>Note: if you use <code>@PersistenceUnit</code> instead of <code>@PersitenceContext</code> it will not work.</div><div><br /></div><div>You will need to have methods in the <code>BusinessRules</code> class in order to access your data. Here is an example of how to select a person using their primary key (continued from the code above):<br />
<pre>public Person getPerson(String personId) {
 Person aPerson = manager.find(Person.class, personId);
 return aPerson;
}</pre></div>

<div> After you do this you will just need to call an instance of your <code>BusinessRules</code> class (<code>businessRules</code>) and then call methods (e.g. <code>businessRuless.getPerson("1");)</code></div> In order to get an Object <code>Person</code> as an entity from your database based on their username and password you will want to overload your method in <code>BusinessRules</code> (or just not use the former method since it isn't very practical) and write a query using EJB QL. Example (continued from code above):</div>

<pre>public Person getPerson(String userid, String password) {
 Query q = manager.createQuery("select p from Person p where p.userid = :uid and p.password = :pass");
 q.setParameter("uid", userid);
 q.setParameter("pass", password);
 Person aPerson = (Person) q.getSingleResult();
 return aPerson;
}</pre>

<div>Resources: <a href="http://java.sun.com/javaee/6/docs/tutorial/doc/giqpj.html">Sun</a>, Enterprise JavaBeans 3.0 5th Edition 2006 Bill Burke &amp; Richard Monson-Haefel</div>