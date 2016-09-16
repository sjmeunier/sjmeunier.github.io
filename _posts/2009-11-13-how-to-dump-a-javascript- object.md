---
layout: post
title:  "How to Dump a Javascript Object"
date:   2009-11-15 00:00:00
categories: Programming
tags: [javascript, tutorials]
---

When trying to debug Javascript, objects can be a real pain, since trying to determine the value of the entire contents of the object is not entirely easy. You can access individual components of the object is quite straightforward, but more often than not, the structure of the whole object is much more useful than individual components.

Trying to find a way to do this, I found a [blog](http://weblogs.asp.net/skillet/archive/2006/03/23/440940.aspx) mentioning a way to do this, and then modified the function quite a bit to suite my own purposes.

To use the function you just need to call the function **dumpObject** with the object and the maximum depth as the parameters. The function then outputs a string containing the formatted contents of the object.

The function basically just recurses through all the items in the supplied object until it either reaches the bottom or else reaches the maximum depth.
<!--more-->

{% highlight javascript %}
function dumpObject(obj, maxDepth) {
	var dump = function(obj, name, depth, tab){
		if (depth > maxDepth) {
			return name + ' - Max depth\n';
		}

		if (typeof(obj) == 'object') {
			var child = null;
			var output = tab + name + '\n';
			tab += '\t';
			for(var item in obj){
				child = obj[item];
				if (typeof(child) == 'object') {
					output += dump(child, item, depth + 1, tab);
				} else {
					output += tab + item + ': ' + child + '\n';
				}
			}
		}
		return output;
	}

	return dump(obj, '', 0, '');
}
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 13 Nov 2009_