---
layout: post
title:  "Maths Algorithms in C#: The Median"
date:   2009-10-21 00:00:03
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

The median is defined as the data point that falls exactly at the midpoint of a set of data points. If there is an even number of points, then the average is taken of the middle two points.

There is one consideration though – the data points sent through to this function needs to be sorted, otherwise it will return garbage data.

{% highlight csharp %}
public static double Median(List<double> values)
{
	int mid;
	double median;

	values.Sort();

	if (MathExt.IsEven(values.Count))
	{
		mid = (values.Count / 2) - 1;
		median = (values[mid] + values[mid + 1]) / 2.0;
	}
	else
	{
		mid = (values.Count / 2);
		median = values[mid];
	}

	return median;
}
{% endhighlight %}

The full sourcecode for the MathLib library is available at [https://github.com/sjmeunier/mathlib](https://github.com/sjmeunier/mathlib)

_Originally posted on my old blog, Smoky Cogs, on 23 Oct 2009_

_Updated 5 Oct 2016: Updated code snippet after refactoring MathLib library_