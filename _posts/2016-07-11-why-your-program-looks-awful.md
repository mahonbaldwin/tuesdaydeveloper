---
layout: post
title:  "Why Your Program Looks Awful"
date:   2016-07-11 12:34:56 -0700
categories: design agile
---
A cautionary tale of application development.

## Sprint 1

#### story 1 (feature)

**Estimation:** 5

**Specification:** Create a plane figure with three straight lines of equal length that meet at the ends forming a total of three interior equiangular angles.

### Sprint 1 Result

![Sprint 1 Result]({{site.url}}/assets/11jul2016-sprint1.png)

A plane figure with three relatively straight lines with one line only slightly longer than the other two forming three interior angles, close to equal. We are only human, we can’t draw perfectly.

We receive accolades for a sprint well done.


## Sprint 2

#### story 2 (bug)

**Estimation:** 5

**Specification** The longer line needs to be shortened to be equal to the other two, lines still need to meet. Oh, and can you make it blue?


#### story 3 (feature)

**Specification** We need to have a fourth side added to our figure.

**Estimation:** 2

### Sprint 2 Result
![Sprint 2 Result]({{site.url}}/assets/11jul2016-sprint2.png)

A figure that looks like that of the first (with a few lines erased and redrawn leaving some artifacts of old code) one of the lines is now blue. On the top there is a fourth side protruding (like a tangent) that does not connect to anything. _In the defense of the programmers, this is a perfectly valid response to the specifications that were given._



## Sprint 3

#### story 4 (bug)

**Specification** Connect the end of line three with the end of line four.

**Estimation:** 2

### Sprint 3 Result

![Sprint 3 Result]({{site.url}}/assets/11jul2016-sprint3.png)

It looks like a rhombus, with a few lines erased.

We relieve hesitant pats on the back right after they ask why we only got 2 points of work done when we got 7 done last spring and 5 the sprint before.

## ad infinitum

This results in Sprint 4 which asks to make all connecting lines perfectly perpendicular (because what they wanted the whole time was a square), and the process continues.




## Programming vs. Building Cars

Building programs is a lot like building cars—you have to smash a few to make sure the rest are safe. We rarely understand what a program should look like from the beginning. It takes a lot of deliberate thought and patience before the design of an application emerges. Sometimes you have to try and fail so you know how to try again. People do this in other fields too. Successful entrepreneurs will tell you that their first 50 businesses were failures. Published authors will speak of their many, many rejection letters. Artists, rarely become famous overnight. Sometimes programmers will have to try out one design before they realize its flaws.

But programming is also like raising children. Parents _may not_ smash their children! Even if they mess up. If you mess up somehow (maybe you lose your temper and yell) you have to work with the result. Sometimes the application you have in production is what you have to work with and you can't just start over. But you're still going to make mistakes, so what do you do?

This is what agile is all about. You get to define the parameters over how frequently you might fail but not whether you will fail. Sometimes you have to refactor greatly (e.g. even throw away part of your code) so that the program delivered is the program being asked for. The initial architecture of an application may not be what you will want to end up with and so you have to change it. But you have to work with what you've got now to produce what you really need.

{% highlight math %}
x = current application

f(x) => x'
{% endhighlight %}

You'll notice that there were lines that didn't get completely erased in the example above. These should have been removed, but we ran out of sprint and so they'll probably live there until some poor soul comes along and wonders what on earth this is supposed to do and he asks a lot of questions about the code and he maybe (in a fit of bravery) removes it, is relieved that all the tests pass, but he doesn't sleep well for a couple nights because he's afraid that he'll get a call reporting some error in the application due to the code that didn't seem to do anything....

## How to fix it

Like many things the only thing we can do is be careful and attentive in our work. We have to take time to think and design _before_ we start coding. There's a great quote I heard recently "I find that weeks of coding and testing can save me hours of design."[^design] We have to double check our work after we commit, and it's really helpful if we take time to make the code we just finished easier to read. Ask your neighbor if he understands the code in its current state. Refactor until you think it's easier to understand.

[^design]:I'm sorry I couldn't find a source for this.

## Problems out of our hands

There are problems that are implicit to the tools we use or the domain we're in. We should try to reduce complexity as much as possible. Use a better tool if possible, but there will still be some things that will be complicated about our application whether we like it or not. I still believe that we should refactor to simplicity as much as we can, knowing that there may be a limit.

## It's only going to be used once

If your application is only going to be used one time to parse a million records from one format and input them into another, maybe you don't have to follow these rules. If there is even a chance that you or anyone else will have to read the code again, I urge you to take some time to save time down the road.