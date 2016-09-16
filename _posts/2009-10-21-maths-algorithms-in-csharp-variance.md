---
layout: post
title:  "Maths Algorithms in C#: Variance"
date:   2009-10-21 00:00:05
categories: Programming
tags: [c-sharp, tutorials, maths]
---

I talked a bit about variance in the post on [standard deviation](/programming/2009/10/21/maths-algorithms-in-csharp-standard-deviation.html), so this is a little bit a rehash from what I said there.

The variance is obtained by calculating the data set consisting of each data point in the original data set subtracting the arithmetic mean for the data set, and then squaring it.

We then take the arithmetic mean of the deviations, which gives us the variance for our data set.

{% highlight csharp %}
public static double Variance(double[] data, int items)  
{  
    int i;  
    double variance;  
    double[] deviation = new double[items];  
  
    mean = ArithmeticMean(data, items);  
  
    for (i = 0; i < items; i++)  
    {  
        deviation[i] = Math.Pow((data[i] - mean), 2);  
    }  
  
    variance = ArithmeticMean(deviation, items);  
  
    return variance;  
}  
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 21 Oct 2009_