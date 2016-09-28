---
layout: post
title:  "Faster String Concatenation in JavaScript"
date:   2011-04-20 00:00:00
categories: Programming
tags: [javascript, tutorials, programming, web]
---

With AJAX being the norm for websites these days, and JavaScript taking on more a more central role in building up web content, you are more than likely going to find yourself having to build up strings containing html to display in a webpage using JavaScript.

This does come at a price though. String concatenations can be rather slow, especially if there is a lot of data that needs to get processed, such as a large table. Unsurprisingly, this is most noticeable in Internet Explorer.

A colleague of mine came up with an alternative way of creating the new html string, using an array to build it up instead of a string.

So using the traditional method you would write:

{% highlight javascript %}
var htmlStr = '<table>';
for(i = 0; i < data.count; i++){
  htmlStr += '<tr><td>' + data[i] + '</td></tr>';
}
htmlStr += '</table>';
document.getElementById(elemId).innerHTML = htmlStr;
{% endhighlight %}

Using an array though, this can be rewritten as

{% highlight javascript %}
var htmlArray = [];
htmlArray.push('<table>');
for(i = 0; i < data.count; i++){
  htmlArray.push('<tr><td>');
  htmlArray.push(data[i]);
  htmlArray.push('</td></tr>');
}
htmlArray.push('</table>');
document.getElementById(elemId).innerHTML = htmlArray.join('');
{% endhighlight %}

In some tests, my colleague found that speed improvements of up to 80x-400x were shown using the array method, varying on the complexity of the concatenations. The biggest improvements were shown in Internet Explorer, with less gains measured in Firefox and Chrome, although quick results were measured in each one.

_Originally posted on my old blog, Smoky Cogs, on 20 Apr 2011_
