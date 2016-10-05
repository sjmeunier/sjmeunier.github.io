---
layout: post
title:  "Maths Algorithms in C#: Linear Least Squares Fit"
date:   2009-10-21 00:00:06
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

When analysing data it is often useful to find an equation to show the relationship in a certain set of data. The linear least squares fit tries to do exactly that.

Given a set of data points, it tries to find a straight line with the equation y = Mx + B which best fits the data, using the least squares technique, which works out the squares of the deviations of each point and then tries to minimize those.

{% highlight csharp %}
public static void LeastSquaresFitLinear(List<Point> points, ref double m, ref double b)
{
	double x1, y1, xy, x2, j;

	x1 = y1 = xy = x2 = 0.0;

	foreach(Point point in points)
	{
		x1 += point.X;
		y1 += point.Y;
		xy += point.X * point.Y;
		x2 += point.X * point.X;
	}

	j = (points.Count * x2) - (x1 * x1);
	if (j != 0.0)
	{
		m = ((points.Count * xy) - (x1 * y1)) / j;
		b = ((y1 * x2) - (x1 * xy)) / j;
	}
	else
	{
		m = 0;
		b = 0;
	}
}
{% endhighlight %}

The full sourcecode for the MathLib library is available at [https://github.com/sjmeunier/mathlib](https://github.com/sjmeunier/mathlib)

_Originally posted on my old blog, Smoky Cogs, on 23 Oct 2009_

_Updated 5 Oct 2016: Updated code snippet after refactoring MathLib library_