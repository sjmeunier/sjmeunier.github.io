---
layout: post
title:  "Drawing Spirals in C#"
date:   2013-03-02 00:00:00
categories: Programming
tags: [c-sharp, tutorials, graphics, programming]
---

Spirals are a relatively easy shape to draw, but in order to draw a good spiral we need a bit of simple trigonometry.

The basics of the spiral are the radius of a particular point from the origin, at a particular angle, and for the code below, the radius increases as the angle increases. The exact relation between angle and radius determines the type of spiral.

In the simplest case, the radius will increase linearly with the angle, thus
_Radius = Angle * ScalingFactor_

We can also use quadratic or cubic equations to define the realtionship
_Radius = Angle2 * ScalingFactor_
_Radius = Angle3 * ScalingFactor_
<!--more-->

The most interesting spiral, however, is the exponential spiral, which is found in nature most famously in the nautilus shell.
_Radius = Anglee * ScalingFactor_

Now that you can determine the relationship between radius and angle, it is a simple matter to draw the spiral.

Starting at the origin, with angle 0, we need to increment the angle by a certain amount – in the code below by 0.5 degrees per iteration – and then calculate the radius, and then using the radius and the angle, we are able to calculate the x and y coordinates of the point by using simple trigonometry, since sin(angle) = y/r and cos(angle) = x/r.

Now after finding the coordinates of the point, we simply need to draw a line segment from our previous point to the new point. The smaller the angle increment, the smoother the curve which is drawn will be, but it also means that more points need to be calculated to draw the same curve, which consumes more processing time.

One good way of speeding up the calcution of the curve, is to use a lookup table for the cos and sin 

{% highlight csharp %}
  public void drawSpiral(double scale, double delta, double revolutions, int centreX, int centreY, SpiralType spiralType, int width, int height, Color color, Graphics g)
  {
	 Pen p = new Pen(Color.Blue, 1);

	 double prevX = centreX;
	 double prevY = centreY;
	 double X = centreX;
	 double Y = centreY;
	 double theta = 0;
	 double radius = 0;

	 while (theta <= (revolutions * 360))
	 {
		theta += delta;
		if (spiralType == SpiralType.Linear)
		{
		   radius = theta * scale;
		}
		else if (spiralType == SpiralType.Quadratic)
		{
		   radius = theta * theta * scale;
		}
		else if (spiralType == SpiralType.Cubic)
		{
		   radius = theta * theta * theta * scale;
		}
		else if (spiralType == SpiralType.Exponential)
		{
		   radius = (Math.Pow(theta / 180 * Math.PI, Math.E)) * scale;
		}

		prevX = X;
		prevY = Y;
		X = (radius * Math.Cos(theta / 180 * Math.PI)) + centreX;
		Y = (radius * Math.Sin(theta / 180 * Math.PI)) + centreY;
		g.DrawLine(p, (float)prevX, (float)prevY, (float)X, (float)Y);
	 }

  }

  public enum SpiralType
  {
	 Linear,
	 Quadratic,
	 Cubic,
	 Exponential
  }
{% endhighlight %}


_Originally posted on my old blog, Smoky Cogs, on 2 Mar 2013_
