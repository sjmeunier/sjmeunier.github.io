---
layout: post
title:  "Finding the Prime Factors of a Number in C&#35;"
date:   2009-10-23 00:00:06
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

A prime number is any number that can only be evenly divided by itself and 1. So for example 2, 3 and 5 are prime, but 6 and 9 are not.

Now, to find all the prime numbers that are factors in a number, we need to first fcheck if the number is even. If it is , then 2 is a factor, and then we keep dividing until the number is odd.

Then, starting at 3, and counting up in 2′s, we try and find a number which divides into the number, each time dividing by the divisor if a match is found.

{% highlight csharp %}
using System.Collections;  
  
public static int[] PrimeFactors(int num)  
{  
   ArrayList factors = new ArrayList();  

   bool alreadyCounted = false;  
   while (num % 2 == 0)  
   {  
	   if (alreadyCounted == false)  
	   {  
		   factors.Add(2);  
		   alreadyCounted = true;  
	   }  
	   num = num / 2;  
   }  

   int divisor = 3;  
   alreadyCounted = false;  
   while (divisor <= num)  
   {  
	   if (num % divisor == 0)  
	   {  
		   if (alreadyCounted == false)  
		   {  
			   factors.Add(divisor);  
			   alreadyCounted = true;  
		   }  
		   num = num / divisor;  
	   }  
	   else  
	   {  
		   alreadyCounted = false;  
		   divisor += 2;  
	   }  
   }  

   int[] returnFactors = (int[])factors.ToArray(typeof(int));  

   return returnFactors;  
}  
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 23 Oct 2009_