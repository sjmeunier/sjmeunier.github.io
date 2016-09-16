---
layout: post
title:  "Maths Algorithms in C#: The Arithmetic Mean"
date:   2009-10-21 00:00:01
categories: Programming
tags: [c-sharp, tutorials, maths]
---

The arithmetic mean is defined as the average of a set of data.  It is found by taking the sum of each value in the data set, and then dividing by the total number of data points.

{% highlight csharp %}
public static double ArithmeticMean(double[] data, int items)  
{  
    int i;  
    double mean, sum;  
  
    sum = 0.0;  
  
    for (i = 0; i < items; i++)  
    {  
        sum  += data[i];  
    }  
        
    mean =  sum / (double)items;  
    return mean;  
}  
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 21 Oct 2009_