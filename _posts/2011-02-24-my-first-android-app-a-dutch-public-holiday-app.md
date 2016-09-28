---
layout: post
title:  "My First Android App - A Dutch Public Holiday App"
date:   2011-02-24 00:00:00
categories: Programming
tags: [android, java, programming]
---

Having my own Android-based phone now, I have been unable to resist the temptation to try my hand at writing an app for it. Deciding on what app I should write, however, required a bit of thinking.

Firstly, if you have ever browsed the Android Market (and if you own an Android phone, no doubt you have), you will have noticed that it has literally hundreds of thousands of applications available for download (either free or for purchase), so finding an application idea that hasn&#8217;t been done a hundred times before becomes a bit of a challenge.

The second thing, is that my Java is a little bit on the rusty side. The last time I have written anything at all in Java was around 11 years ago, and even then my jaunt with Java was rather brief.

This means that to ease myself into the Java development environment of Android, I wanted to go with a relatively simple app, leaving more ambitious projects for later.

So, out of this, I came up with the Dutch Public Holidays app, which all it basically does is give you a dropdown of years (currently only 2011 and 2012), and then displaying the public holidays in the Netherlands for that particular year. Very, very simple!

The actual code of the app is largely based on the [spinner tutorial](http://developer.android.com/resources/tutorials/views/hello-spinner.html") on the Android Developer site, but instead of updating a toast, the application updates the TextView with the info we want, and I rearranged how some of the code worked.

The source for the project is available at [https://github.com/sjmeunier/dutchpublicholidays](https://github.com/sjmeunier/dutchpublicholidays)

I intend to write more apps to learn more about Android, and at some stage get my apps onto the Android Market, but that is something for another day&#8230;

_Originally posted on my old blog, Smoky Cogs, on 24 Feb 2011_
