---
layout: post
title:  "Iterating Over Char Values in C#"
date:   2010-10-17 00:00:00
categories: Programming
tags: [c-sharp, tutorials]
---

In C&#35;, the **char** data type represents a text character. Internally though, this value is pretty much a numeric value.

Think here of Ascii or Unicode values. These codings essentially map a character to a particular numeric value, which is what the **char** data type stores.

One rather interesting side effect of this is that you can iterate through character values in exactly the same way as you would for an **int** or **long**.

In the sample code below, the code loops through all the capital letters of the alphabet, adding each letter to a string.

{% highlight csharp %}
string alphabet = "";
for (char c = 'A'; c <= 'Z'; c++)
{
    alphabet += c.ToString();
}

{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 17 Oct 2010_
