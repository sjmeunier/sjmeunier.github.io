---
layout: post
title:  "Finding the Greatest Common Devisor in C#"
date:   2009-10-23 00:00:06
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

The greatest common divisor of two numbers is the largest number that divides evenly into both numbers.

The method of calculating this is is to use repeated division and subtraction.

Given two numbers _a_ and _b_, the first step is to repeatedly divide both numbers by 2 as long as both are even, keeping count of the number of iterations as _d_.

Now, we loop again while _a_ is not equal to _b_. If either of the numbers are even we divide the number by 2, without incrementing _d_ in this case. If both numbers are odd, we then replace the bigger of _a_ or _b_ with the difference of the two numbers divided by 2.

Eventually the two numbers will be equal and the loop exits.

Now at this point the greatest common divisor is given by _gcd = a (or b) * 2<sup>d</sup>_

{% highlight csharp %}
public static int GreatestCommonDivisor(int a, int b)
{
	if (a <= 0 || b <= 0)
		throw new Exception("Parameters need to be greater than 0");
	int d = 0;
	while ((a % 2 == 0) && (b % 2 == 0))
	{
		a = a / 2;
		b = b / 2;
		d += 1;
	}

	while (a != b)
	{
		if (a % 2 == 0)
			a = a / 2;
		else if (b % 2 == 0)
			b = b / 2;
		else if (a > b)
			a = (a - b) / 2;
		else
			b = (b - a) / 2;
	}
	int gcd = a * ((int)Math.Pow(2, d));

	return gcd;
}
{% endhighlight %}

The full sourcecode for the MathLib library is available at [https://github.com/sjmeunier/mathlib](https://github.com/sjmeunier/mathlib)

_Originally posted on my old blog, Smoky Cogs, on 23 Oct 2009_

_Updated 5 Oct 2016: Updated code snippet after refactoring MathLib library_