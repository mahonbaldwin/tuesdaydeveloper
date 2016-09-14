---
layout: post
title:  ""
date:   2016-08-25 15:34:56 -0700
categories: cheapskate travel mac chromebook
---
Five things to know about me:
1. I'm a cheapskate.
1. I really like Macs.
1. I travel occasionally for work.
1. I can't count

You see, I've been waiting for the new MacBook Pros since February. And in my haste to wait,
I've had time to think. I assumed that as soon as the new MacBook Pros were finally available
I'd get one. And that might still be true, but there were only two things I needed:
1. More than 16GB RAM (I run VMs once in a while and IntelliJ is a RAM hog)
1. Portability

I'm guessing that the new MacBook Pros will not come with a 32GB RAM option, but you can
get an iMac and get that. Of course if I were willing to use Linux I could go with [System76](http://system76.com). But,
well I won't go there.

But unless I want to lug around a hugh iMac when I travel, that wouldn't work. Of course,
if I could get an iMac and use it at home and a MacBook and use that when I travel that would
be awesome, also twice as expensive.

So what do I do? Well I come up with my own solution. (It's your fault Apple, you told me
to "Think Different™" so that's what I'm gonna do.)

What if I use an iMac at home and a Chromebook[^mb-air] to log into my home computer when I'm out?

[^mb-air]: I could use a MacBook Air to do the same thing, but if I'm concerned over price
this is a more feasable solution.

Maybe, the purpose of this post is to explore this possibility. 
You see, there are several problems with this. Some of them have answers.

## Q: How do I even do that?
A: There are several remote desktop clients. Some of them are even free. I'm going to use

## Q: What if I need to restart my computer?
A: If you have FileVault disabled, just restart like normal, but if you do have your drive
encrypted Apple has a utility for that. `sudo fdesetup authrestart` will ask for your
password and bypass the FileVault login screen and allow your remote desktop client
to start and then you'll just login like normal.

## Q: What if the computer has an error and shuts down or restarts on its own?
Maybe the power goes out or the computer crashes.

A: Erm, I'm not sure. I can use a battery backup to keep my computer up in most cases, 
but other than that I'm stumped.

I hope that if this happens my wife will be home or I can call a
neighbor to restart my computer and log me in.

One possible answer, if you don't have FileVault enabled is to schedule your mac in
System Preferences > Energy Saver > Schedule... to wake or turn on every day. You may have
to wait until the next time for the computer to start, but it might be the answer.

## Q: What about my special keyboard shortcuts?
A: Well, the search button on Chromebooks is supposed to be the same as the Command Key.
We'll see.

## Q: What if I don't want anyone to see what my remote computer is doing?
A: Well, I'm not really concerned by that. If I'm out of town and my wife wants to 
watch me code on my computer she can, but if this does concern you, there are some
remote desktop clients that have a "curtain mode" so the remote computer is blank. 
