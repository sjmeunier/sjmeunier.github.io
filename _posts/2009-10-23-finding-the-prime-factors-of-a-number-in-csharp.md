---
layout: post
title:  "Finding the Prime Factors of a Number in C&#35;"
date:   2009-10-23 00:00:06
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

A prime number is any number that can only be evenly divided by itself and 1. So for example 2, 3 and 5 are prime, but 6 and 9 are not.

Now, to find all the prime numbers that are factors in a number, we need to first check if the number is even. If it is, then 2 is a factor, and then we keep dividing until the number is odd.

Then, starting at 3, and counting up in 2′s, we try and find a number which divides into the number, each time dividing by the divisor if a match is found.

{% highlight csharp %}
using System.Collections;  
  
public static List<long> PrimeFactors(long val)
{
	List<long> factors = new List<long>();

	while (val % 2 == 0)
	{
		factors.Add(2);
		val = val / 2;
	}

	long divisor = 3;
	while (divisor <= val)
	{
		if (val % divisor == 0)
		{
			factors.Add(divisor);
			val = val / divisor;
		}
		else
		{
			divisor += 2;
		}
	}
   
	return factors;
}
{% endhighlight %}

The full sourcecode for the MathLib library is available at [https://github.com/sjmeunier/mathlib](https://github.com/sjmeunier/mathlib)

_Originally posted on my old blog, Smoky Cogs, on 23 Oct 2009_

_Updated 5 Oct 2016: Updated code snippet after refactoring MathLib library_