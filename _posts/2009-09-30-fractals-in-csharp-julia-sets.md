---
layout: post
title:  "Fractals in C#: Julia Sets"
date:   2009-09-30 00:00:07
categories: Programming
tags: [c-sharp, tutorials, fractals, programming]
---

Julia sets are very similar to Mandelbrot sets, and if you have not yet looked at my post on generating [Mandelbrot sets](/programming/2009/09/fractals-in-csharp-mandelbrot-set), then I suggest you take a look. Most of the background is the same.

![Julia Set](/assets/images/blog/fractals/juliaset.jpg){: .shadow-image .centered }

The only major difference is between Julia sets and the Mandelbrot set is that the Mandelbrot iteration is based on the equation **Z** = **Z**^(Power 1) + **Z**^(Power 2) + **C**, where **C** is the point **Z(0)**. The Julia set is based on the equation **Z**^(Power 1) - **Z**^(Power 2) - **L**, where **L** is a constant complex number.

We iterate through each point, using the centre of the screen as the origin (0,0). For each point then, we iterate up to our maximum depth, using the Julia formula now instead of the Mandelbrot one, and break when the absolute value of **Z** reaches 2.

We can then draw the point, using the same method as we used for drawing the Mandelbrot point.
<!--more-->

{% highlight csharp %}
public void Generate(Graphics g, int iIterations, double Scaling, int iInitialSize, Complex L, double iOffsetRe, double iOffsetIm, int iLeft, int iTop, int iWidth, int iHeight, int iPower, int iPower2)
{
	Complex Z = new Complex( 0.0, -0.0);
	Complex Offset = new Complex( 0.0, -0.0);

	// iterate over the area in the complex plane indicated by the Scaling and Offset
	//   i is the real axis,  j is the complex axis

	for (double i = -iWidth/2; i < iWidth/2 ; i++)
	{
		for (double j = -iHeight/2; j < iHeight/2; j++)
		{
			// Normalize Z and adjust for Scaling and Offset
			Z.re = (i/((double)iInitialSize)) * Scaling + (double)iOffsetRe;
			Z.im = (j/((double)iInitialSize)) * Scaling + (double)iOffsetIm;

			// C is the Z(0) of the formula based on the pixel position
			// We've also added a ManualOffset from the text boxes on the screen

			int iteration = 0;

			for (int k = 1; k < iIterations; k++)
			{
				Z = (Z^iPower) - (Z^iPower2) - L;
				// if modulus of Z > 2, break out of the loop
				// and remember the current iteration to choose the color
				if (Z.Abs() > 2.0)
				{
					iteration = k;
					break;
				}
			}

			DrawComplexPoint(g, ((int)i) + (iWidth/2), ((int)j) + (iHeight/2), iteration);
		}
	}
}

{% endhighlight %}

The full source code is available at [https://github.com/sjmeunier/fractalize](https://github.com/sjmeunier/fractalize).

_Originally posted on my old blog, Smoky Cogs, on 30 Sep 2009_
