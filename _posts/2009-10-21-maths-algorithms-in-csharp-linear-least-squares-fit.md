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
public static void LeastSquaresFitLinear(Pnt[] points, int numPoints, ref double M, ref double B)  
{  
    //Gives best fit of data to line Y = MC + B  
    double x1, y1, xy, x2, J;  
    int i;  
  
    x1 = 0.0;  
    y1 = 0.0;  
    xy = 0.0;  
    x2 = 0.0;  
  
    for (i = 0; i < numPoints; i++)  
    {  
        x1 = x1 + points[i].X;  
        y1 = y1 + points[i].Y;  
        xy = xy + points[i].X * points[i].Y;  
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