---
layout: post
title: "Google App Engine Datastore"
date: 2010-02-20 17:11:49.0
author: mahon
categories: 
---
Storing and retrieving persistent data on the Datastore is <em>very</em> easy. I'll walk you through the steps of each.

<strong>Set up</strong>
Before you can store data, you will need a PersistenceManager (javax.jdo.PersistenceManager). The PersistenceManager implements the singleton pattern (so only one instance can be open at any given time). Google recommends using a singleton wrapper to get the programs PersistenceManager. The following code if found in <code>PMF.class:</code>
<div class="codeblock">
<pre lang="java" line="1">import javax.jdo.JDOHelper;
import javax.jdo.PersistenceManagerFactory;

public final class PMF {
private static final PersistenceManagerFactory pmfInstance = JDOHelper.getPersistenceManagerFactory("transactions-optional");

private PMF() {
}

public static PersistenceManagerFactory get() {
     return pmfInstance;
}
}</pre>
</div>
To use this file, when you need data you will need to get the current instance of it using this code:<code>PersistenceManager pm = PMF.get().getPersistenceManager();</code>

<strong>Creating JDOs</strong>
Now that we have our PMF class, we will need to create an entity bean. An entity bean is a simple Java class with attributes and getters and setters (to get and store data). There are a few special tags that you must remember when creating these beans to use in Google App Engine.

<strong><code>@PersistenceCapable(identityType = IdentityType.APPLICATION)</code></strong> if you forget this one, you won't be able to store anything.

<strong><code>@PrimaryKey
@Persistent(valueStrategy = IdGeneratorStrategy.IDENTITY)</code></strong> these are for your primary key. You can generate your own primary keys if you want (use email addresses or whatever) but I generally like to stick with the keys that the datastore provide.

<strong><code>@Persistent</code></strong> this must go in front of all the attributes you want to store (or the datastore will conveniently forget to store them.

Now lets look at an example: This is a simple class with only two attributes, a Key and a String.
<div class="codeblock">
<pre lang="java" line="1">import javax.jdo.annotations.IdGeneratorStrategy;
import javax.jdo.annotations.IdentityType;
import javax.jdo.annotations.PersistenceCapable;
import javax.jdo.annotations.Persistent;
import javax.jdo.annotations.PrimaryKey;

import com.google.appengine.api.datastore.Key;

@PersistenceCapable(identityType = IdentityType.APPLICATION)
public class FavoriteFood {
        //attributes
        @PrimaryKey
        @Persistent(valueStrategy = IdGeneratorStrategy.IDENTITY)
        private Key key;

	@Persistent
	private String foodName;

        //auto generated getters and setters (all are present except setKey() which is unneeded
	public String getFoodName() {
		return foodName;
	}

	public void setFoodName(String foodName) {
		this.foodName = foodName;
	}

	public Key getKey() {
		return key;
	}
}</pre>
</div>
<strong>Setting data</strong>
Now that we have our PMF class and a JDO to work with, we can now store data. Google App Engine only allows data to be stored programmatically so storing data is essential before retrieval is possible. While there are many ways to store data, I use AJAX to send a request to a FrontController object which forwards all requests to an ApplicationController which determines which class to run. All the classes implement an interface I call Interactor. Click here for information about the <a href="http://tuesdaydeveloper.com/?p=97">FrontController, ApplicationController, and the Iterator</a>.

It is quite simple to store data:
<div class="codeblock">
<pre lang="java" line="1">// ... PersistenceManagare pm = PMF.get().getPersistenceManager();
                FavoriteFood aFood = new FavoriteFood();
		aFood.setFoodName(foodName);

                try {
                        pm.makePersistent(aFood);
                } finally {
                        pm.close();
                }</pre>
</div>
Thats it! You may have to do the logic to check it the <code>Object</code> you are persisting is valid, but if you want to store something, it's pretty simple. Editing a persistent <code>Object</code> is even easier, well ... about as easy. But before we can do that, we have to get the <code>FavoriteFood</code> out of storage (lets hope it keeps well).

<strong>Getting persistent data</strong>
There are several ways to get an <code>Object</code> from storage. I'll show you two methods, if you like SQL you'll like the first, if you prefer method calls then you'll like the second. I'll also show you the method to get an <code>Object</code> by its <code>Key</code>.
<div class="codeblock">
<pre lang="java" line="1">//... PersistenceManager pm = PMF.get().getPersistenceManager();
try {
     // First declare a query String
     String query = "select from " + FavoriteFood.class.getName();
     // or if you want to filter (don't forget the single quotes around the filter)
     String queryFilter = "select from " + FavoriteFood.class.getName() + " where foodName =='" + foodName + "' order by foodName";

     // Then use the persistence manager to get the instances of the Object
     List foods = (List) pm.newQuery(query).execute();

     // then do whatever you want with the results
     // you may want to check if the results are empty: if(foods.isEmpty()) {...
     // you may want to iterate through the results: for(Food f: foods) {...
} finally {
     pm.close();
}</pre>
</div>
This next approach is even easier.
<div class="codeblock">
<pre lang="java" line="1">//... PersistenceManager pm = PMF.get().getPersistenceManager();
Query query = pm.newQuery(FavoriteFood.class);
try {
     List foods = query.execute();
} finally {
     pm.close();
}</pre>
</div>
To set filter data run this after you declare the query: <code>query.setFilter("favoriteFood == favoriteFoodVar")</code> followed by the parameter declaration: <code>query.declareParameters("String favoriteFoodVar")</code>. When you run the query you will enter the favoriteFoodVar in as a parameter: <code>query.execute("ice cream");</code>. To set an order to the results, before you run the query type: <code>query.setOrdering("favoriteFood desc");</code>

To get an <code>Object</code> by its <code>Key</code> you may need a few things. The <code>KeyFactory</code> may be helpful. One of its uses is to convert a <code>Key</code> to a <code>String</code> and back if necessary. This is helpful with form submission etc.:
<div class="codeblock">
<pre lang="java" line="1">//change a Key to a String
String foodKeyString = KeyFactory.keyToString(foodKey);

//change a String to a Key
Key foodKey = KeyFactory.stringToKey(foodKeyString);
</pre>
</div>
Once you have a <code>Key</code> you can easily get any object using the <code>getObjectById(Class, Key)</code> method.
<div class="codeblock">
<pre lang="java" line="1">// PersistanceManager is open as "pm"
FavoriteFood food = pm.getObjectById(FavoriteFood.class, foodKey);
</pre>
</div>
<strong>Editing persistent data</strong>
When you have data in the datastore that you want to change, it is very simple. To edit an <code>Object</code> that is already in the datastore you just need to open the <code>PersistenceManager</code> and get the object you want to to edit. Once it is open you can just change it: <code>object.setParamName("whatever")</code> then close the PersistenceManager like normal in the <code>finally</code> block:
<div class="codeblock">
<pre lang="java" line="1">// make sure the PersistanceManager is open
try {
     FavoriteFood food = pm.getObjectById(FavoriteFood.class, foodKey);
     food.setFoodName("enchiladas");
} finally {
     //once the PersistanceManager is closed, the data is made permanent.
     pm.close();
}</pre>
</div>
<strong> Deleting data</strong>
Deleting persistant data is probably one of the easiest thing you can do. Once you have the object you just need to run the deletePersistent(Object o) method
<div class="codeblock">
<pre lang="java" line="1">// PersistanceManager is open
try {
     FavoriteFood food = pm.getObjectById(FavoriteFood.class, foodKey);
     pm.deletePersistent(food);
} finally {
     pm.close();
}</pre>
</div>
<strong>Conclusion</strong>
So now you know how to create data, get that content, edit content, and delete content.

All of this can be found on Google's <a href="http://code.google.com/appengine/docs/java/datastore/creatinggettinganddeletingdata.html">Creating, Getting and Deleting Data</a> page, the <a href="http://code.google.com/appengine/docs/java/datastore/dataclasses.html">Defining Data Classes</a> page, or the <a href="http://code.google.com/appengine/docs/java/datastore/queriesandindexes.html">Queries and Indexes</a> page.

<div class='archived comments'>

<div class='comment'>[...] Terms of Use      &laquo; Google App Engine Datastore [...]  <div class='by'>tuesdayDeveloper; &raquo; Blog Archive &raquo; FrontController, ApplicationController, and Interactor on 2010-02-20 17:45:26.0  </div></div>
<div class='comment'>[...] (you&#8217;ll also do similar things with other Objects). Note, you may want to read my post on Google App Engine Datastore to get some of the basics. code block&nbsp;&nbsp;&nbsp;//...//open [...]  <div class='by'>tuesdayDeveloper; &raquo; Blog Archive &raquo; Query Objects not just Strings on 2010-03-22 16:00:08.0  </div></div>
<div class='comment'>Very usefull!!

//change a Key to a String
String foodKeyString = KeyFactory.keyToString(foodKey);
 
//change a String to a Key
Key foodKey = KeyFactory.stringToKey(foodKeyString);

but.....
if i have:
long id = foodKey.getId(); //for example 22002

It's possible get the foodKeyString from id?

Thank's Janka
from Italy  <div class='by'>Janka on 2010-09-24 13:08:52.0  </div></div>
<div class='comment'>Janka,

Yes, you can get that information, but you'll need to do a query to get it. Something like the following:

<pre><code>Query query = pm.newQuery(FavoriteFood.class);
query.setFilter("favoriteFood == favoriteFoodParam");
query.declareParameters("long favoriteFoodIDParam")
try {
     List foods = query.execute(favoriteFoodID);//favoriteFoodID needs to be a long and be declared earlier
     if(!foods.isEmpty()) {
          Food theFood = usersWithUserId.iterator().next();//this assumes that you are searching for one
     }
} finally {
     pm.close();
}
return theFood.getId();</code></pre>  <div class='by'>Mahon on 2010-09-24 18:48:08.0  </div></div>
</div>