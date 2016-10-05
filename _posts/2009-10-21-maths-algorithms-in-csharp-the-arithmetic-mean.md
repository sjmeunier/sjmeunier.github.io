---
layout: post
title:  "Maths Algorithms in C#: The Arithmetic Mean"
date:   2009-10-21 00:00:01
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

The arithmetic mean is defined as the average of a set of data.  It is found by taking the sum of each value in the data set, and then dividing by the total number of data points.

{% highlight csharp %}
public static double ArithmeticMean(List<double> values)
{
	double mean, sum;

	sum = 0.0;
	foreach (double value in values)
	{
		sum += value;
	}

	mean =  sum / (double)values.Count;
	return mean;
}
{% endhighlight %}

The full sourcecode for the MathLib library is available at [https://github.com/sjmeunier/mathlib](https://github.com/sjmeunier/mathlib)

_Originally posted on my old blog, Smoky Cogs, on 23 Oct 2009_

_Updated 5 Oct 2016: Updated code snippet after refactoring MathLib library_