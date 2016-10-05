---
layout: post
title:  "Solving Quadratic Equations in C#"
date:   2009-10-23 00:00:04
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

Finding the roots of a quadratic equation (which is where it crosses the x axis) can be a rather involved calculation. I remember in school having to do them, and the equations they gave usually worked out to nice numbers. Often in the real world, it is not quite so easy.

Fortunately, computers are not scared by numbers that are difficult, and can solve quadratics quite well.

The function below solves quadratics in the form of Ax<sup>2</sup> + Bx + C = 0. Providing A, B and C, the function finds the two roots of the equation, and also determines whether the roots are real, real repeating or complex roots.

{% highlight csharp %}
public static void SolveQuadratic(double a, double b, double c, ref double x1Real, ref double x1Im, ref double x2Real, ref double x2Im, ref Enums.RootType rootType)
{
	double z = 0;

	z = Math.Pow(b, 2) - (4 * a * c);
	
	if (Math.Round(z, 2) < 0)
	{
		x1Real = -1 * b / (2.0 * a);
		x2Real = x1Real;

		x1Im = Math.Sqrt(Math.Abs(z)) / (2.0 * a);
		x2Im = -1 * x1Im;

		rootType = Enums.RootType.Complex;
	}
	else if(Math.Round(z, 2) == 0)
	{
		x1Real = -1 * b / (2.0 * a);
		x2Real = x1Real;

		x1Im = 0.0;
		x2Im = 0.0;

		rootType = Enums.RootType.Repeating;
	}
	else
	{
		x1Real = ((-1 * b) + Math.Sqrt(z)) / (2.0 * a);
		x2Real = ((-1 * b) - Math.Sqrt(z)) / (2.0 * a);

		x1Im = 0.0;
		x2Im = 0.0;

		rootType = Enums.RootType.Real;
	}
}
{% endhighlight %}

The full sourcecode for the MathLib library is available at [https://github.com/sjmeunier/mathlib](https://github.com/sjmeunier/mathlib)

_Originally posted on my old blog, Smoky Cogs, on 23 Oct 2009_

_Updated 5 Oct 2016: Updated code snippet after refactoring MathLib library_