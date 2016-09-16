---
layout: post
title:  "Maths Algorithms in C#: The Geometric Mean"
date:   2009-10-21 00:00:02
categories: Programming
tags: [c-sharp, tutorials, maths]
---

The geometric mean is similar to the arithmetic mean in that it is a type of average for the data, except it has a rather different way of calculating it.

The geometric mean is defined as the n-th root of the product of each value in the data set, where n is the number of data points in the set. This makes it useful for describing things such as percentage growth.

{% highlight csharp %}
public static double GeometricMean(double[] data, int items)  
{  
    int i;  
    double GMean, prod;  
  
    prod = 1.0;  
  
    for (i = 0; i < items; i++)  
    {  
        prod  *= data[i];  
    }  
    GMean =  Math.Pow(prod, (1.0 / (double)items));  
    return GMean;  
}  
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 21 Oct 2009_