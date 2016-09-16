---
layout: post
title:  "Maths Algorithms in C#: Least Squares Fit With a Log Ordinate"
date:   2009-10-21 00:00:08
categories: Programming
tags: [c-sharp, tutorials, maths]
---

We have already looked at the linear least squares fit, but that does not always produces a nice fit to the data. The least squares fit with a log ordinate tries to match up the data to the equation y = B * 10<sup>M<sup>x</sup></sup> using the least squares method.

In the code below, if there is no solution, the function returns M and B as 0.

{% highlight csharp %}
public static void LeastSquaresFitLogOrdinate(Pnt[] points, int numPoints, ref double M, ref double B)  
{  
    //Gives best fit of data to curve Y = B*(10^M)^X  
  
    double x1, y1, xy, x2, J, LY;  
    int i;  
  
    x1 = 0.0;  
    y1 = 0.0;  
    xy = 0.0;  
    x2 = 0.0;  
  
    for (i = 0; i < numPoints; i++)  
    {  
        LY = Math.Log10(points[i].Y);  
        x1 = x1 + points[i].X;  
        y1 = y1 + LY;  
        xy = xy + points[i].X * LY;  
        x2 = x2 + points[i].X * points[i].X;  
    }  
  
    J = ((double)numPoints * x2) - (x1 * x1);  
    if (J != 0.0)  
    {  
        M = (((double)numPoints * xy) - (x1 * y1)) / J;  
        M = Math.Floor(1.0E3 * M + 0.5) / 1.0E3;  
        B = ((y1 * x2) - (x1 * xy)) / J;  
        B = Math.Floor(1.0E3 * B + 0.5) / 1.0E3;  
    }  
    else  
    {  
        M = 0;  
        B = 0;  
    }  
}  
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 21 Oct 2009_