---
layout: post
title:  "Maths Algorithms in C#: Variance"
date:   2009-10-21 00:00:05
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

I talked a bit about variance in the post on [standard deviation](/programming/2009/10/21/maths-algorithms-in-csharp-standard-deviation.html), so this is a little bit a rehash from what I said there.

The variance is obtained by calculating the data set consisting of each data point in the original data set subtracting the arithmetic mean for the data set, and then squaring it.

We then take the arithmetic mean of the deviations, which gives us the variance for our data set.
<!--more-->

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
		
public static double Variance(List<double> values)
{
	double standardDeviation, variance;

	standardDeviation = StandardDeviation(values);
	variance = Math.Pow(standardDeviation, 2);

	return variance;
}
{% endhighlight %}

The full sourcecode for the MathLib library is available at [https://github.com/sjmeunier/mathlib](https://github.com/sjmeunier/mathlib)

_Originally posted on my old blog, Smoky Cogs, on 23 Oct 2009_

_Updated 5 Oct 2016: Updated code snippet after refactoring MathLib library_