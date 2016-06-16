---
layout: post
title: "FrontController, ApplicationController, and Interactor"
date: 2010-02-20 17:45:23.0
author: mahon
categories: 
---
I mentioned in the post: <a href="http://tuesdaydeveloper.com/?p=94">Google App Store Datastore</a> that I make data persistent by making requests to a FrontController which forwards the request to an ApplicationController which calls the needed class which implements Interactor. This is important for several reasons: (1) It is more secure, there is only one servlet to access my server. (2) Because we can call any class using an ApplicationController and an <em>Interactor</em>.

The following is the code for each of these classes:
<strong>FrontController</strong>
<pre lang="JAVA">@SuppressWarnings("serial")
public class FrontController extends HttpServlet {
	public void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException {
		ApplicationController ac = new ApplicationController(req, resp); //calls ApplicationController
	}

	public void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException {
		this.doGet(req, resp); //doing this will allow you to call the same method
                                                   //whether a developer uses GET or POST
	}
}</pre>
<strong>ApplicationController</strong>
<pre lang="JAVA">public class ApplicationController {
	HashMap controls = new HashMap();

	private void putControl() { //Key is what the application will call, Class is the program to return
		controls.put("addfood", AddFood.class);//do this with all the classes you may need to call
	}

	public ApplicationController(HttpServletRequest request, HttpServletResponse response) {
		putControl();
		String action = request.getParameter("a").toLowerCase();//in the url string, ?a=addfood&amp;...

		Class aClass = controls.get(action);

		try {
			if (aClass != null) {
				Interactor anInteractor = (Interactor) aClass.newInstance();
				anInteractor.invoke(request, response);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}</pre>
<strong>Interactor</strong>
<pre lang="JAVA">import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public interface Interactor {
	void invoke(HttpServletRequest request, HttpServletResponse response);
}</pre>
Note: Classes that implement the Interactor must implement the invoke method. Method signature looks like <code>public class AddFood implements Interactor</code>

<div class='archived comments'>

<div class='comment'>[...] of Use      &laquo; Google App Engine (Java) FrontController, ApplicationController, and Interactor [...]  <div class='by'>tuesdayDeveloper; &raquo; Blog Archive &raquo; Google App Engine Datastore on 2010-02-20 17:51:49.0  </div></div>
</div>