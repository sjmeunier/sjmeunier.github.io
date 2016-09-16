---
layout: post
title:  "Maths Algorithms in C#: Least Squares Fit With a Log Abcissa"
date:   2009-10-21 00:00:09
categories: Programming
tags: [c-sharp, tutorials, maths]
---

This algorithm for the least squares fit uses a logarithmic abscissa, and tries to find an equation matching the data set with the form y = M * log(X)/log(10) + B, using the least squares method.

The function below finds M and B, and if no solution exists, it returns 0 for both these values.

{% highlight csharp %}
public static void LeastSquaresFitLogAbscissa(Pnt[] points, int numPoints, ref double M, ref double B)  
{  
    //Gives best fit of data to curve Y = M*log(X)/log(10) + B  
  
    double x1, y1, xy, x2, J, LX;  
    int i;  
  
    x1 = 0.0;  
    y1 = 0.0;  
    xy = 0.0;  
    x2 = 0.0;  
  
    for (i = 0; i < numPoints; i++)  
    {  
        LX = Math.Log10(points[i].X);  
        x1 = x1 + LX;  
        y1 = y1 + points[i].Y;  
        xy = xy + points[i].Y * LX;  
        x2 = x2 + LX * LX;  
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