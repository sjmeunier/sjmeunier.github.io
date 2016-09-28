---
layout: post
title:  "Maths Algorithms in C#: Standard Deviation"
date:   2009-10-21 00:00:04
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

Standard deviation is a very common calculation in statistical analysis, and is formally defined as the square root of the variance of the data set.

The variance of a data set is how much the data varies from the mean.

So then, in simple terms, what we do is for each data point in the data set, we take the value at that point, subtracting the mean from it, and then squaring it.

Once we have the data set of deviations, we can then find the arithmetic mean of this, which is the variance.

The final step here is to find the squareroot of this which gives us the standard deviation.

{% highlight csharp %}
public static double StandardDeviation(double[] data, int items)  
{  
    int i;  
    double SD, mean, devMean;  
    double[] deviation = new double[items];  
      
    mean = ArithmeticMean(data, items);  
  
    for (i = 0; i < items; i++)  
    {  
        deviation[i] = Math.Pow((data[i] - mean), 2);  
    }  
  
    devMean = ArithmeticMean(deviation, items);  
    SD = Math.Sqrt(devMean);  
  
    return SD;  
}  
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 21 Oct 2009_