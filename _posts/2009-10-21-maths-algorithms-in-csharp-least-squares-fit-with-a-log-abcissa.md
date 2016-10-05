---
layout: post
title:  "Maths Algorithms in C#: Least Squares Fit With a Log Abcissa"
date:   2009-10-21 00:00:09
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

This algorithm for the least squares fit uses a logarithmic abscissa, and tries to find an equation matching the data set with the form y = M * log(X)/log(10) + B, using the least squares method.

The function below finds M and B, and if no solution exists, it returns 0 for both these values.

{% highlight csharp %}
public static void LeastSquaresFitLogAbscissa(List<Point> points, ref double m, ref double b)
{
	double x1, y1, xy, x2, j, lx;

	x1 = y1 = xy = x2 = 0.0;

	foreach (Point point in points)
	{
		lx = Math.Log10(point.X);
		x1 += lx;
		y1 += point.Y;
		xy += point.Y * lx;
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