---
layout: post
title:  "Google Charts API and Scientific Equations"
date:   2009-11-12 00:00:00
categories: Science
tags: [science, google, web]
---

Trying to properly show an equation on a webpage is not exactly the easiest thing to do, since it often relies on arcane symbols and odd formatting. Never fear, Google is to the rescue.

[Google Charts](http://code.google.com/apis/chart/) allows you to embed many types of graphs and charts in a webpage using a simple call to the chart api.

That may be wonderful in itself, but there is an even better use for this API, and that is to render equations. All you need to know for that is a little Latex.

This is an undocumented feature so the documentation from Google on this is rather scarce, although it is rather easy to use. As an example, here is a formula for combinations
![Chart](http://chart.apis.google.com/chart?cht=tx&amp;&amp;chf=bg,s,FFFFFF00&amp;chco=000000&amp;chl=%5C[%5Cleft(%5C!%5C!%5C!%5Cbegin{array}{c}n%20%5C%5Cr%5Cend{array}%5C!%5C!%5C!%5Cright)%20=%20{n}C_r%20=%20%5Cfrac{n!}{r!(n-r)!}%5C])

and the code required to create it is

{% highlight html %}
<img src="http://chart.apis.google.com/chart?cht=tx&chf=bg,s,FFFFFF00&chco=000000&chl=\[\left(\!\!\!\begin{array}{c}n\\r\end{array}\!\!\!\right) = {n}C_r = \frac{n!}{r!(n-r)!}\]">
{% endhighlight %}

The _chf_ parameter sets the background colour of the resulting image, and _chco_ controls the foreground colour. The actual latex code is specified in the _chl_ parameter, which can be any valid latex code. The possibilities for this are endless.

Thanks to [Ryan Moulton](http://moultano.blogspot.com/2009/11/google-can-generate-your-equations-for.html) who put up a blog post on this same topic where I first saw this feature mentioned.

_Originally posted on my old blog, Smoky Cogs on 12 Nov 2009_