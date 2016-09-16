---
layout: post
title:  "Complex Numbers"
date:   2009-10-23 00:00:03
categories: Programming
tags: [c-sharp, tutorials, maths]
---

Complex numbers are a vital part of many mathematical calculations, yet Visual Studio does not natively support complex numbers, but fortunately, this is a very easy oversight to fix.

First, a little maths lesson. Imaginary numbers are numbers that do not exist, like for example the square root of -1. This number is denoted by _i_ and is central to complex numbers. Complex numbers are defined as a number that is made up of an imaginary component as well as a real component, and is denoted by the formula x*_i_ + y.

So some examples of complex numbers are 5_i_ + 4, 2_i_ + 0 (which simplifies to 2_i_), and 0_i_ + 6 (which simplifies to simply 6).

What the above example shows as well, is that the range of complex numbers also includes all the real numbers.

The complex number class contains two variables to contain the real and imaginary components, namely, **re** and **im**. Most of the rest of the class are overloaded arithmetic operators.

The ^ operator takes a complex number and raises it to a specified power, returning a new complex number.

The + and - operators add and subtract two complex numbers, which entails adding or subtracting the real and imaginary parts of the one to the real and imaginary parts of the other complex number.

The * and / operators are a bit more complicated in how they are calculated, but the net result is the multiplication and division of the complex numbers.

The **Abs()** function returns the absolute value of the complex number, which is the square root of the sum of the squares of the real and imaginary components - very much like the absolute value of a normal vector.

Lastly, the **Arg()** function returns the arg of the complex number in degrees, which allows the complex number to be expressed in polar notation.
<!-- more-->

{% highlight csharp %}
namespace MathLib
{

    public class Complex
    {
        public double re;
        public double im;

        public Complex(double pdReal, double pdImaginary)
        {
            re = pdReal;
            im = pdImaginary;
        }

        public static Complex operator ^(Complex arg1, int arg2)
        {
            int i = 0;
            Complex x = new Complex(0.0, 0.0);

            if (arg2 == 0)
            {
                return x;
            }
            else
            {
                x = arg1;
                for (i = 1; i < arg2; i++)
                {
                    x = x * arg1;
                }
                return x;
            }
        }

        public static Complex operator +(Complex arg1, Complex arg2)
        {
            return (new Complex(arg1.re + arg2.re, arg1.im + arg2.im));
        }

        public static Complex operator -(Complex arg1)
        {
            return (new Complex(-arg1.re, -arg1.im));
        }

        public static Complex operator -(Complex arg1, Complex arg2)
        {
            return (new Complex(arg1.re - arg2.re, arg1.im - arg2.im));
        }

        public static Complex operator *(Complex arg1, Complex arg2)
        {
            return (new Complex(arg1.re * arg2.re - arg1.im * arg2.im, arg1.re * arg2.im + arg2.re * arg1.im));
        }

        public static Complex operator /(Complex arg1, Complex arg2)
        {
            double c1, c2, d;
            d = arg2.re * arg2.re + arg2.im * arg2.im;
            if (d == 0)
            {
                return (new Complex(0, 0));
            }
            c1 = arg1.re * arg2.re + arg1.im * arg2.im;
            c2 = arg1.im * arg2.re - arg1.re * arg2.im;
            return (new Complex(c1 / d, c2 / d));
        }

        public double Abs()
        {
            return (Math.Sqrt(re * re + im * im));
        }

        //Arg of complex number in degrees
        public double Arg()
        {
            double ret = 0;
            if (re != 0)
                ret = (180 / Math.PI) * Math.Atan(im / re);
            return (ret);
           
        }

        public override string ToString()
        {
            return (String.Format("Complex: ({0}, {1})", re, im));
        }

        public string ToComplexString(int iRounding)
        {
            string ComplexNumber = Convert.ToString(Math.Round(re, iRounding)) + "+" + Convert.ToString(Math.Round(im, iRounding)) + "i";
            return ComplexNumber;
        }
    }
}

{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 23 Oct 2009_