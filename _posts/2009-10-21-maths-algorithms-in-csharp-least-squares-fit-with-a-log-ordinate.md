---
layout: post
title:  "Maths Algorithms in C#: Least Squares Fit With a Log Ordinate"
date:   2009-10-21 00:00:08
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

We have already looked at the linear least squares fit, but that does not always produces a nice fit to the data. The least squares fit with a log ordinate tries to match up the data to the equation y = B * 10<sup>M<sup>x</sup></sup> using the least squares method.

In the code below, if there is no solution, the function returns M and B as 0.

{% highlight csharp %}
public static void LeastSquaresFitLogOrdinate(List<Point> points, ref double m, ref double b)
{
	double x1, y1, xy, x2, j, ly;

	x1 = y1 = xy = x2 = 0.0;

	foreach (Point point in points)
	{
		ly = Math.Log10(point.Y);
		x1 += point.X;
		y1 += ly;
		xy += point.X * ly;
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