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
public static void SolveQuadratic(double A, double B, double C, ref double X1Real, ref double X1Im, ref double X2Real, ref double X2Im, ref int Type)  
{  
    //Type: 1 = real, 2 = repeating, 3 = complex  
    double Z = 0;  
  
    Z = Math.Pow(B, 2) - (4 * A * C);  
    Z = Math.Floor(100 * Z + 0.5) / 100.0;  
      
    if (Z < 0)  
    {  
        X1Real = (-1) * B / (2.0 * A);  
        X1Real = Math.Floor(100 * X1Real + 0.5) / 100.0;  
        X2Real = X1Real;  
  
        X1Im = Math.Sqrt(Math.Abs(Z)) / (2.9 * A);  
        X1Im = Math.Floor(100 * X1Im + 0.5) / 100.0;  
        X2Im = (-1) * X1Im;  
  
        Type = 3;  
    }  
    else if(Z == 0)  
    {  
        X1Real = (-1) * B / (2.0 * A);  
        X1Real = Math.Floor(100 * X1Real + 0.5) / 100.0;  
        X2Real = X1Real;  
  
        X1Im = 0.0;  
        X2Im = 0.0;  
  
        Type = 2;  
    }  
    else  
    {  
        X1Real = (((-1) * B) + Math.Sqrt(Z)) / (2.0 * A);  
        X1Real = Math.Floor(100 * X1Real + 0.5) / 100.0;  
  
        X2Real = (((-1) * B) - Math.Sqrt(Z)) / (2.0 * A);  
        X2Real = Math.Floor(100 * X2Real + 0.5) / 100.0;  
  
        X1Im = 0.0;  
        X2Im = 0.0;  
  
        Type = 1;  
    }  
}  
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 23 Oct 2009_