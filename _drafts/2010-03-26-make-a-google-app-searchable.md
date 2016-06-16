---
layout: post
title: "Make a Google App Searchable"
date: 2010-03-26 09:03:20.0
author: mahon
categories: 
---
It seemed a bit backwards to me that, given Google's primary consumer service, that the Datastore doesn't allow a full-text search. According to <a href="http://groups.google.com/group/google-appengine/browse_thread/thread/f64eacbd31629668">some sources</a> Google is working on this. In the meen time, we have to use other tools. I tired to to find how to implement google.appengine.ext.search, but there isn't much documentation on that. I did, however, find an <a href="http://www.kimchy.org/searchable-google-appengine-with-compass/">excellent screen cast</a> that instructs how to use Compass, an open source full text search tool written in Java. Below are the steps covered in the tutorial:
Note: I've made some slight alterations to the process because it made more sense to me. I also have some small differences because I had a different Compass build than Kimchy.
<ol>
	<li><a href="http://compass-project.org/">Download Compass and Dependancies</a>
You'll need  Compass (the API to interact with Lucene) and Lucene (an Apache project that implements full-text search). In the screen cast, Kimchy (the tutorialist, is that a word) downloaded the latest nightly build, because I'm developing for a production environment, I'm going to stick with the latest stable build.</li>
	<li>Add dependancies into the project library
Open the folder that downloaded and drag-and-drop the following files into the project [Project Folder]/WEB-INF/lib/
<ul>
	<li><code>compass-[build number][-M3].jar</code> found at [unzipped folder]/dist/</li>
	<li><code>commons-logging.jar</code> fount at [unzipped folder]/dist/</li>
	<li><code>lucene-core.jar</code> found at [unzipped folder]/dist/lucene/</li>
</ul>
</li>
	<li>Before our project can use these, we need to add the libraries to the class path.
Right-click the project folder, and go to Properties &gt; Java Build Path (in the list on the left) &gt; Libraries (button on the top). Then click on the Add JARs button on the top right. Navigate to all three of the jar files you just added to your lib folder and add them.</li>
	<li>Configure Compass
Open the <code>PMF.class</code> (<a href="http://tuesdaydeveloper.com/?p=94">see instructions for the PersistenceManagerFactory</a>) and edit it to look as follows:
[codesyntax lang="java" lines="normal" lines_start="1"]
<pre>public final class PMF {
	private static final PersistenceManagerFactory pmfInstance = JDOHelper.getPersistenceManagerFactory("transactions-optional");

	<strong>private static final Compass compass;

	private static final CompassGps compassGps;

	static {
		compass = new CompassConfiguration().setConnection("gae://index")
		.setSetting(CompassEnvironment.ExecutorManager.EXECUTOR_MANAGER_TYPE, "disabled")
		.addScan("us.honco.prs4.beans")
		.buildCompass();

		compassGps = new SingleCompassGps(compass);
		compassGps.addGpsDevice(new JpaGpsDevice());</strong>//in Kimchy's screencast compassGps.addGpsDevice(new Jdo2GpsDevice("appengine", pmfInstance)<strong>
		compassGps.start();

		compassGps.index();
	}</strong>

    private PMF() {
    }

    <strong>public static Compass getCompass() {
    	return compass;
    }</strong>

    public static PersistenceManagerFactory get() {
        return pmfInstance;
    }
}</pre>
[/codesyntax]</li>
	<li>Make your beans searchable
Mark the bean with the annotation <code>@Searchable</code> (you'll need to import <code>org.compass.annotations.Searchable</code>) and each of the attributes that you want indexed. See the Example below.
[codesyntax lang="java" lines="normal" lines_start="1" container="div"]
<pre>@PersistenceCapable(identityType = IdentityType.APPLICATION)
@Searchable
public class Person {
	@PrimaryKey
        <strong>@SearchableId</strong> //if you want your ids to be searchable
        @Persistent(valueStrategy = IdGeneratorStrategy.IDENTITY)
        private Key personKey;

	@Persistent
        <strong>@SearchableProperty</strong>
	private String username;

        @Persistent
        <strong>@SearchableProperty(formate = "yyyy-MM-dd")</strong> //add a formate
        private Date addDate;

        @SearchableProperty //you can also make getters searchable, though I don't really see the point since we can make the attributes searchable
        public getUsername() {
            return username;
        }</pre>
[/codesyntax]</li>
	<li>We're almost done, next we need to make a search.jsp page. Why, you ask, well if we don't  then the Compass is never used. ;) The search page should have something like the following:</li>
</ol>