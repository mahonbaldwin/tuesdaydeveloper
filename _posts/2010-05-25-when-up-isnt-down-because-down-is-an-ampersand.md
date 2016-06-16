---
layout: post
title: "When Up isn't Down Because Down is an Ampersand"
date: 2010-05-25 20:28:22.0
author: mahon
categories: 
---
I'm working on a project that I'm building in <a href="http://code.google.com/appengine/">Google App Engine</a> with <a href="http://code.google.com/webtoolkit/">Google Web Toolkit</a>. On part of the the page I'm creating I have a number box. The user can type in a number or to the side of the number box there is an up arrow and a down button. In addition to typing and the up and down buttons I also wanted the user to be able to press the up and down button on the keyboard to increment or decrement the box. This was very strait forward—but I hit a small snag. To include this code I added a listener to the box called <code>activeNumber</code>:
<pre lang="java" line="101">	    activeNumber.addKeyPressHandler(new KeyPressHandler() {
	    	public void onKeyPress(KeyPressEvent event) {
	    		int charCode = event.getCharCode();
	    		int nativeCode = event.getNativeEvent().getKeyCode();
	    		if(charCode == KeyCodes.KEY_UP) {
	    			incrementActiveNumber();
	    		} else if(charCode == KeyCodes.KEY_DOWN) {
	    			decrementActiveNumber();
	    		} else if(charCode == KeyCodes.KEY_BACKSPACE) {
	    		} else if(nativeCode < 48 || nativeCode > 57) {
	    			event.preventDefault();
	    		}
	    	}
	    });</pre>
You'll see that I also wanted to prevent any character (other than numbers) to be entered into the box, any nativeCode that is less than 48 or greater than 57 (0-9) were disabled, but for some reason when I tried it, I could type in an ampersand [&] and an open parentheses [(]. I tried for the longest time to figure out why these characters were allowed. I did a lot of editing in the last if statement, but didn't seem to be effective until I noticed that when I hit the ampersand, the activeNumber would increment and when I would push the open parentheses it would decrement. It was then that I determined that the KeyCode for the up button is 38 and the down button is 40. I then looked up the key values for the ampersand and the open parentheses, 38 and 40.

How did I fix it? I simply disabled the default for up and down as follows:
<pre lang="java" line="101">	    activeNumber.addKeyPressHandler(new KeyPressHandler() {
	    	public void onKeyPress(KeyPressEvent event) {
	    		int charCode = event.getCharCode();
	    		int nativeCode = event.getNativeEvent().getKeyCode();
	    		if(charCode == KeyCodes.KEY_UP) {
	    			event.preventDefault();
	    			incrementActiveNumber();
	    		} else if(charCode == KeyCodes.KEY_DOWN) {
	    			event.preventDefault();
	    			decrementActiveNumber();
	    		} else if(charCode == KeyCodes.KEY_BACKSPACE) {
	    		} else if(nativeCode < 48 || nativeCode > 57) {
	    			event.preventDefault();
	    		}
	    	}
	    });</pre>

Bwa-ha-ha, try to type in an ampersand now!