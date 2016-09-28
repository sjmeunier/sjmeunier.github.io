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
public static void LeastSquaresFitLogFull(Pnt[] points, int numPoints, ref double M, ref double B)  
{  
    //Gives best fit of data to curve Y = B*X^M  
  
    double x1, y1, xy, x2, J;  
    double[] LX = new double[numPoints];  
    double[] LY = new double[numPoints];  
    int i;  
  
    x1 = 0.0;  
    y1 = 0.0;  
    xy = 0.0;  
    x2 = 0.0;  
  
    for (i = 0; i < numPoints; i++)  
    {  
        LX[i] = Math.Log10(points[i].X);  
        LY[i] = Math.Log10(points[i].Y);  
        x1 = x1 + LX[i];  
        y1 = y1 + LY[i];  
        xy = xy + LY[i] * LX[i];  
        x2 = x2 + LX[i] * LX[i];  
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