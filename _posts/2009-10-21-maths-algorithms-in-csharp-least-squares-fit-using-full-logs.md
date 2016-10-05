---
layout: post
title:  "Maths Algorithms in C#: Least Squares Fit Using Full Logs"
date:   2009-10-21 00:00:10
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

We have already looked at several other ways of doing a least squares fit to find an quation representing a set of data. We now look at the least squares fit using full logs, which tries to match the data with the equation y = B*x<sup>M</sup>, using the least squares method.

As with the previous least squares functions, the function below returns the calculated values for M and B, and if no solution exists returns 0 for both of them.

{% highlight csharp %}
public static void LeastSquaresFitLogFull(List<Point> points, ref double m, ref double b)
{
	double x1, y1, xy, x2, j, lx, ly;

	x1 = y1 = xy = x2 = 0.0;

	foreach (Point point in points)
	{
		lx = Math.Log10(point.X);
		ly = Math.Log10(point.Y);
		x1 += lx;
		y1 += ly;
		xy += ly * lx;
		x2 += lx * lx;
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