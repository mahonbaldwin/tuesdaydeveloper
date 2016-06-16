---
layout: post
title:  "Clojure vs. x"
date:   2016-06-06 20:00:42 -0700
categories: clojure programming-style
---
A week or so ago I was interviewing a candidate to come to work with my company. After briefly explaining why we chose Clojure for our application we asked him if he had any questions about our architecture. "Well," his eyes flicked to the side before returning eye contact, "why not just use Java?"

## Why should I choose...?
I'm fascinated by the question, "Why should I choose _x_ programming language/paradigm/framework/parrot?" Obviously this can be a silly question because a bird that can program can make you a lot of money even if its not very _good_ at programming! As for the non-fowl-programming-entities, you'll get a lot more debate as to which has the most merit.

I've been drawn to Clojure for a while now. Clojure is a functional, dynamically-typed, Lisp dialect. I can list for you the air of grievances people have against Clojure even knowing only the short description I provided:

* What's wrong with OO?
* No types???!!! Seriously?
* Ugh, the parenthesis!

Objections by others asside, I like it and a lot of other people like it too. [Clojure comes highly recommended](https://www.thoughtworks.com/radar/languages-and-frameworks/clojure). It's even in [the top 100 most popular languages](http://www.tiobe.com/tiobe_index)[^tiobe]. But no one can doubt that for one reason or another Clojure is less popular than Java, C#, Ruby, Python, JavaScript, C, C++, even PHP. So this leads me to the question: what makes a programming language _popular_?

[^tiobe]: As of 10 Jun 2016.

## Hammers
> If all you have is a hammer, everything looks like a nail.
>
> —[Abraham Kaplan or Abraham Maslow](https://en.wiktionary.org/wiki/if_all_you_have_is_a_hammer,_everything_looks_like_a_nail)

I hate hearing that phrase. Whichever Abraham said it, it is the single most quoted text I hear when I discuss functional paradigms with other people. And it wasn't even originally about programming. Is it true?[^hammer] Perhaps the sentiment of the quote is true to some degree. I want to point out that there are cases that for which Clojure is not a good fit. I haven't worked with any, but I know that when working with limited amounts of space or close to the hardware you don't have higher level languages at your disposal.

I'm also not saying that Clojure is the only good language nor that it will never be replaced with something better. It is the nature of our profession to continue learning and creating new technologies because we have more knowledge and resources at our disposal. That means that Clojure will be replaced someday.

I will, however, be bold and say that I think that Clojure is the best tool we have available for most use cases.


[^hammer]:  Well if I'm being pedantic, no, it's not true. You see, nails must be pounded into something like a board, if everything looked like a nail then you'd have to use a hammer to pound nails into nails which doesn't make much sense. Not to mention the hammer would look like a nail too, so it would be more like using a nail to pound nails into nails. (This is starting to sound like a torture scenario, especially if one or more of those nails is really fingernails. Ehh.)



## Language Analysis
To understand how one language is different from another I think that you have to understand the features of a language.

### Language Families
One language feature is that of the manner it is written. As far as I know, there are really only two styles of writing software[^software-styles]. Those styles are C and LISP. C looks like `x = y`. It's often punctuated with semicolons and feels familiar to anyone has been introduced to high school algebra. LISP looks like `(fn x y)` where `fn` is a function and `x` and `y` are arguments. Everything in LISP is written with lists. In fact, LISP is short for List Processor.

[^software-styles]: I am not counting coding straight in binary or even assembly code.

Since everything is lists in LISPs, this makes for extremely simple syntax, because once you understand that the first symbol of a list (something enclosed in `()`) must be a function[^escape-list], you basically have the syntax down. I'm not familiar with other LISPs[^lisps], but in Clojure the only syntax you really need to know are lists `()`, maps `{}`, vectors `[]`, sets `#{}`, and a special feature called keywords that start with `:`[^clj-syntax].

[^escape-list]: You can escape a list so it won't invoke a function allowing you to use the first space for something other than a function, but for that most of the time you can use vectors.

[^lisps]: From what I understand mosts LISPs have even less syntax. Clojure chose to add a few extra forms to ease in code understanding \(which also decreases the number of parentheses by a lot.\)

[^clj-syntax]: There are also special meanings for `#`, <code class="highlighter-rouge">`</code>, `'`, and `~` but you'd be able to read the language just fine even if you didn't know that.

C syntax, on the other hand is more complicated. You usually have a lot of reserved keywords[^keywords] peppered with different punctuation that mean different things based on the context. In Java the `()` in `for(int i = 0; i < 10; i++)` mean something different than they mean in `main(String[] args)` which mean something different than they do in `System.out.print("Hello World")`. You might think I'm being anal about this, because they are all fairly similar, but all of those rules have to be written in a compiler and that adds to the complexity to a system.

[^keywords]: Clojure \(and as far as I know other LISPs\) don't really have reserved keywords. Yes, you have functions that are predefined for you, but they are all namespaced so you can use really any name that consists of alpha-numeric characters, dashes, and underscores even if it's used in the core library, but most people don't.

### Language Styles
There are also different programming _paradigms_. I'm not going to get into all of them[^lang-styles], but you could probably say that the two main ones are Imperative Programming and Declarative Programming. The main difference with the two: With imperative programming you tell the computer not only what do do, but how to do it. With declarative you tell the computer what to do, but let it figure out how.

[^lang-styles]: I'm definitely not qualified to talk about all of them.

These sorts of features are cropping up in several different traditional imperative languages. Take the following examples in JavaScript (with the latest ES2015 update has become way better in it's declarative style).

{% highlight javascript %}
//imperative style
function getSum(arr){
  var sum = 0;
  for(var i = 0; i < arr.length; i++) {
    sum += arr[i];
  }
  return sum;
}
{% endhighlight %}

{% highlight javascript %}
//declarative style
function getSum(arr){
  arr.reduce((preVal, curVal) => preVal + curVal);
}
{% endhighlight %}

{% highlight clojure %}
; the same example in clojure for comparison
(defn sum [arr]
  (reduce + arr))
{% endhighlight %}

When comparing the first style with the subsequent styles, the following quote makes a lot of sense.

> OO makes code understandable by encapsulating moving parts.  FP makes code understandable by minimizing moving parts.
>
> —[Michael Feathers](https://twitter.com/mfeathers/status/29581296216)

In general, functional languages seek to have a more declarative style, it is for this reason that functional languages are often considered more expressive.

With imperative languages you don't have much of a whole language API. What you really have is a lot of micro APIs. `String` has a set of methods, `List` has a set of methods, `YourObject` has a set of methods. Declarative languages tend to have a larger whole language API. In imperative languages you often have separate `.equals()` methods. `Object`'s `.equals()` method is overridden. More often than not, a function that works on a vector will also work on a map _and it's the same function_ in declarative languages like Clojure. This philosophy can be summed up in this quote:

> It is better to have 100 functions operate on one data structure than 10 functions on 10 data structures.
>
> —[Alan Perlis](http://www.cs.yale.edu/homes/perlis-alan/quotes.html)

## How is All Fits
![Languages Table](/assets/lang-table.png)
This image is an approximation of how different languages might fit when you look at language style and family. As you can see it seems a bit crowded in the bottom left corner. Of course, you could say that all of the Imperative-C languages have elements of declarative style and they are certainly gaining aspects of declarative notation. I put them there because the typical coding style for someone using those languages is more imperative. As for Scheme, I'll admit I'm not sure where to put that. I looked on Wikipedia at the Scheme code examples and it looked very imperative to me (which may mean you can write imperative code in any language[^imperativeAnyLang]). In actuality, you'd probably see more languages strataling the line between declarative and imperative.

[^imperativeAnyLang]: I tried it in Clojure, it's possible, but more difficult that just writing idiomatic declarative code.

## Conclusion
So, why not Java? That's an excellent question, one that I'm becoming more and more convinced to be qualified to answer that you need to have a psychology degree because selecting your language of choice seems to be such a personal decision. As so many other preferences, it seems to come down to your values:

