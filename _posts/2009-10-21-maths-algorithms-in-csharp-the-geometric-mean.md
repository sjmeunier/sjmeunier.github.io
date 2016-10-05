---
layout: post
title:  "Maths Algorithms in C#: The Geometric Mean"
date:   2009-10-21 00:00:02
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

The geometric mean is similar to the arithmetic mean in that it is a type of average for the data, except it has a rather different way of calculating it.

The geometric mean is defined as the n-th root of the product of each value in the data set, where n is the number of data points in the set. This makes it useful for describing things such as percentage growth.

{% highlight csharp %}
public static double GeometricMean(List<double> values)
{
	double geometricMean, product;

	product = 1.0;
	foreach(double value in values)
	{
		product *= value;
	}

	geometricMean =  Math.Pow(product, (1.0 / (double)values.Count));
	return geometricMean;
}
{% endhighlight %}

The full sourcecode for the MathLib library is available at [https://github.com/sjmeunier/mathlib](https://github.com/sjmeunier/mathlib)

_Originally posted on my old blog, Smoky Cogs, on 23 Oct 2009_

_Updated 5 Oct 2016: Updated code snippet after refactoring MathLib library_