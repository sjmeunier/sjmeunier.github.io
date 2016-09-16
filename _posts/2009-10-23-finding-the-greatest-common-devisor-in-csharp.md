---
layout: post
title:  "Finding the Greatest Common Devisor in C#"
date:   2009-10-23 00:00:06
categories: Programming
tags: [c-sharp, tutorials, maths]
---

The greatest common divisor of two numbers is the largest number that divides evenly into both numbers.

The method of calculating this is is to use a little bit of recursion.

If the numbers are even, then work out the GCD by dividing both numbers by 2 and passing as parameters to the GCD function recursively, multiplying the result by 2.

If only one number is even, then pass that number divided by 2, and the whole second number to the GCD function to get the GCD.

If both numbers are odd, then call the GCS function with the average of the two numbers and the second number as the parameters.

If the numbers are equal then return that number, and if either number is equal to then return 1.

By the time execution reached the top again after the recursion, you should have the greatest common divisor as your resulting number.

{% highlight csharp %}
public static int GCD(int A, int B)  
{  
    if (A == B)  
    {  
        return A;  
    }  
    if ((A == 1) || (B == 1))  
    {  
        return 1;  
    }  
    if ((A % 2 == 0) && (B % 2 == 0))  
    {  
        return 2 * GCD(A / 2, B / 2);  
    }  
    else if ((A % 2 == 0) && (B % 2 != 0))  
    {  
        return GCD(A / 2, B);  
    }  
    else if ((A % 2 != 0) && (B % 2 == 0))  
    {  
        return GCD(A, B / 2);  
    }  
    else  
    {  
        return GCD(Math.Abs(A - B) / 2, B);  
    }  
}  
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 23 Oct 2009_