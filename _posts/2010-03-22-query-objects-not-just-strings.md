---
layout: post
title: "Query Objects not just Strings"
date: 2010-03-22 16:00:05.0
author: mahon
categories: 
---
Haha, I just thought how weird that title would look to anyone who isn't a programmer (why would you want to ask sting something).

Stupid jokes aside, I thought it would be important to write down how to query <code>Object</code>s, like dates or people in Google App Engine. The concept is very simple, though you may have to put in a little extra thought. For example, lets query a date. We want to design a calendar that will store events for different days, and we want these events to have specific times (that should be obvious). How do we search for these when we need to?

Say we're searching for all events on March 22nd. This will include my "Wake up" alert at 6:30 am as well as my "Appointment with Mr. Money" from 10:00 am to 2:00 pm. In any query language you'll have to say, "Hey, db, I want everything between 12:00 am and 11:59:59 on March 22nd." To do this in Google App Engine you'll do the following, (you'll also do similar things with other <code>Object</code>s). Note, you may want to read my post on <a href="http://tuesdaydeveloper.com/?p=94">Google App Engine Datastore</a> to get some of the basics.

<pre lang="java" line="1">
//...
//open your persistence manager
PersistenceManager pm = PMF.get().getPersistenceManager()
//create a new Query
Query eventQuery = pm.newQuery(Event.class);
//set the filter for the Query. Notice that there is a range the date must fall in, this may or may not be necessary on other Objects.
eventQuery.setFilter("startDate <= startDateParam && startDate <= endDateParam");
//declare the parameters of the Query
eventQuery.declareParameters("java.util.Date startDateParam, java.util.Date endDateParam"); //notice the full address of the Date class
//set any ordering you want
eventQuery.setOrdering("startDate");
//execute the query
List<Event> events = (List<Event>) eventQuery.execute(startDate, endDate);
//...

</pre>

You'll notice that I used java.util.Date even though a great majority of the methods in this class are deprecated. The reason why is because of a bug I came across (I feel okay calling it a bug since other people have had problems with it and since when I changed the class name it made no difference but when I changed my <code>GregorianCalendar</code>s to <code>Date</code>s it worked fine.) Maybe they will fix it and you won't have the same problem.