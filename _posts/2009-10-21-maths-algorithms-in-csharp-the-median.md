---
layout: post
title:  "Maths Algorithms in C#: The Median"
date:   2009-10-21 00:00:03
categories: Programming
tags: [c-sharp, tutorials, maths]
---

The median is defined as the data point that falls exactly at the midpoint of a set of data points. If there is an even number of points, then the average is taken of the middle two points.

There is one consideration though – the data points sent through to this function needs to be sorted, otherwise it will return garbage data.

{% highlight csharp %}
public static double Median(double[] data, int items)  
{  
    int midPoint;  
    double median, sum;  
  
    sum = 0.0;  
  
    if (((int)Math.Round((double)items / 2.0) * 2) != items)  
    {  
        midPoint = items / 2;  
  
  
        sum  = data[midPoint];  
        sum  += data[midPoint + 1];  
        sum /= 2.0;  
    }  
    else  
    {  
        midPoint = (items / 2) + 1;  
        sum = data[midPoint];  
    }  
      
    median = sum;  
    return median;  
}  
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 21 Oct 2009_