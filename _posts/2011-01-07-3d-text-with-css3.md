---
layout: post
title:  "3D Text with CSS3"
date:   2011-01-07 00:00:00
categories: Programming
tags: [css, web, programming]
---

Text shadows are one of the new features quickly becoming popular in CSS3. Mark Dotto has used this technique to create 3D text, which is an incredibly ingenius use for this. His [post](/web/20131231073319/http://www.markdotto.com/2011/01/05/3d-text-using-just-css/) shows how he did it.

The code which Mark wrote to do this shown here, with a few modifications that I made myself, which I will explain in a moment.

{% highlight css %}
h1 {
  text-shadow: 1px 1px 0 #aaa, 
               2px 2px 0 #a9a9a9,
               3px 3px 0 #aaa,
               4px 4px 0 #999999,
               5px 5px 0 #9a9a9a,
               6px 6px 1px rgba(0,0,0,.1),
               6px 0 5px rgba(0,0,0,.1),
               6px 1px 3px rgba(0,0,0,.3),
               6px 3px 5px rgba(0,0,0,.2),
               6px 5px 10px rgba(0,0,0,.25),
               6px 10px 10px rgba(0,0,0,.2),
               6px 20px 20px rgba(0,0,0,.15);
}
{% endhighlight %}

So if you are running Chrome, Firefox, Safari or Opera, the following text should be looking a little less flat than usual.

<style>
.raised {
  font-size: 80px;
  text-shadow: 1px 1px 0 #aaa, 
               2px 2px 0 #a9a9a9,
               3px 3px 0 #aaa,
               4px 4px 0 #999999,
               5px 5px 0 #9a9a9a,
               6px 6px 1px rgba(0,0,0,.1),
               6px 0 5px rgba(0,0,0,.1),
               6px 1px 3px rgba(0,0,0,.3),
               6px 3px 5px rgba(0,0,0,.2),
               6px 5px 10px rgba(0,0,0,.25),
               6px 10px 10px rgba(0,0,0,.2),
               6px 20px 20px rgba(0,0,0,.15);
}
</style>
<h1 class="raised">Hello World</h1>

How he accomplishes the 3D effect is to create multiple shadows, which build up to the 3D effect. The order of the parameters in the above code for each line of the text-shadow css tag are _[x pos] [y pos] [blur radius] [color]_. The first 5 lines create the solid shadow in the 3D effect, by successively creating a shadow. In this example, each successive shadow is moved right and down one pixel from the previous shadow.

The original, by Mark, kept the x value as 0px, which creates a vertical effect as seen on his [demo](http://markdotto.com/playground/3d-text/), while the code above shows a slanted effect.

The height of the raised look is determined by how many shadows you draw, so in this example, we have a height of 5, but depending on the effect you want to go for, you can have more or fewer shadows to create relatively higher or lower raised effects. The next several lines draw in a more diffuse shadow around the text. This is not strictly necessary for a 3D effect, but it gives a nice shadowy look to the text. Here, the blurring radius is used, in addition to a semi-transparent color, to create the diffuse shadow.

Like with the main shadow, the effect here can be varied depending on how many of the diffuse shadows, their transparency and blur radii you use. The possibilities are endless.

_Originally posted on my old blog, Smoky Cogs on 7 Jan 2011_