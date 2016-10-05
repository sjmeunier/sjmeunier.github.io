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
public static double StandardDeviation(List<double> values)
{
	double standardDeviation, mean, deviationMean;
	List<double> deviation = new List<double>();
	
	mean = ArithmeticMean(values);

	foreach(double value in values)
	{
		deviation.Add(Math.Pow((value - mean), 2));
	}

	deviationMean = ArithmeticMean(deviation);
	standardDeviation = Math.Sqrt(deviationMean);

	return standardDeviation;
}
{% endhighlight %}

The full sourcecode for the MathLib library is available at [https://github.com/sjmeunier/mathlib](https://github.com/sjmeunier/mathlib)

_Originally posted on my old blog, Smoky Cogs, on 23 Oct 2009_

_Updated 5 Oct 2016: Updated code snippet after refactoring MathLib library_