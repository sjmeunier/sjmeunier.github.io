---
layout: post
title:  "Fractals in C#: Fern Fractals"
date:   2009-09-30 00:00:03
categories: Programming
tags: [c-sharp, tutorials, fractals]
---

Fern fractals are another class of iterative fractals, a type of iterative function system, that draws rather good images of ferns. And with a bit of tweaking of the input variables, the ferns of various sizes and shapes can be created using the same code.

![Fern fractal](/assets/images/blog/fractals/fernfractal.jpg){: .shadow-image .centered }

To create a fractal fern you first need to supply a rather large amount of input variables that will control the shape of the fern. So in the application, the variables are stored in a few arrays.
<!--more-->

{% highlight csharp %}
	double[] dA = new double[2];
	double[] dB = new double[6];
	double[] dC = new double[6];
	double[] dD = new double[6];
	int[] iRand = new int[4];

	dA[0] = 0;
	dA[1] = 0.16;
	dB[0] = 0.2;
	dB[1] = -0.26;
	dB[2] = 0;
	dB[3] = 0.23;
	dB[4] = 0.22;
	dB[5] = 1.6;
	dC[0] = -0.15;
	dC[1] = 0.28;
	dC[2] = 0;
	dC[3] = 0.26;
	dC[4] = 0.25;
	dC[5] = 0.44;
	dD[0] = 0.85;
	dD[1] = 0.04;
	dD[2] = 0;
	dD[3] = -0.04;
	dD[4] = 0.85;
	dD[5] = 1.6;
	iRand[0] = 1;
	iRand[1] = 8;
	iRand[2] = 15;
	iRand[3] = 85;
{% endhighlight %}

So, now we have arrays dA, dB, dC, dD and iRand. Very simply, the values in dA control the rachis of the fern (the main stem for those, like me, who weren't listening in biology class),  dB controls the left pinna (a'leaflet'), dC controls the right pinna, and dD controls the main shape of a frond (the fern's 'leaf'). iRand contains the probability that a value from each of the four arrays will be used.

Now the main function iterates through a specified number of points (which in the sample on this page, I used 100000 iterations), by first setting the current x and y coordinate to 0, and then it starts the iteration.

A random number is generated between 0 and 1, and if the number is less than iRand[0] then it calculates the new xy coordinate based on the dA array. It then checks each probability through all the probabililities, each time calculating the xy coordinate using the array and calculation specified at that value.

Once the new coordinates are calculated, the point is drawn using the desired colour, and then the function loops to the next iteration, using these coordinates as the base coordinates for the next iteration.

Once the function has iterated enough times, the function exits, and you should be left with a decent fern.

Half the fun of this type of fractal is playing around with the input variables to see how they affect the output, so have fun. 

{% highlight csharp %}
public void GenerateFern(Graphics g, int iIterations, double dStartX, double dStartY, double dScale, double[] dA, double[] dB, double[] dC, double[] dD, int[] iRand, Color oColor)
{
	FastRandom rnd = new FastRandom();
	int iRandNum;
	double dX = 0;
	double dY = 0;
	double dNewX = 0;
	double dNewY = 0;

	for (int i = 0; i < iIterations; i++)
	{

		iRandNum = rnd.Next(0, 100);
		if (iRandNum < iRand[0])
		{
			dNewX = dA[0];
			dNewY = dA[1] * dY;

		}
		else if (iRandNum < iRand[1])
		{
			dNewX = (dB[0] * dX) + (dB[1] * dY) + dB[2];
			dNewY = (dB[3] * dX) + (dB[4] * dY) + dB[5];
		}
		else if (iRandNum < iRand[2])
		{
			dNewX = (dC[0] * dX) + (dC[1] * dY) + dC[2];
			dNewY = (dC[3] * dX) + (dC[4] * dY) + dC[5];
		}
		else
		{
			dNewX = (dD[0] * dX) + (dD[1] * dY) + dD[2];
			dNewY = (dD[3] * dX) + (dD[4] * dY) + dD[5];
		}
		dX = dNewX;
		dY = dNewY;
		g.FillRectangle(new SolidBrush(oColor), (float)(dStartX + (dNewX * dScale)), (float)(dStartY - (dNewY * dScale)), 1, 1);
	}

}
{% endhighlight %}

The full source code is available at [https://github.com/sjmeunier/fractalize](https://github.com/sjmeunier/fractalize).

_Originally posted on my old blog, Smoky Cogs, on 30 Sep 2009_
