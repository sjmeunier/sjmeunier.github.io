---
layout: post
title:  "Using jQuery to Find JavaScript Events"
date:   2012-05-31 00:00:00
categories: Programming
tags: [javascript, tutorials, jquery, programming, web]
---

One of the most frustrating things about JavaScript is being able to determine what events are being fired for a particular element, especially in a complex web application where a single element might have several different functions attached to a particular event.

Well, jQuery comes to the rescue.

jQuery has a very useful, and rather unknown, method to return all the events attached to an element, which is particularly useful when used in the web browser’s console while debugging a web page.
<!--more-->

All this needs is one single line of code, and hey presto!

{% highlight javascript %}
$(element).data('events');
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 31 May 2012_
