---
layout: post
title:  "Combinations, Permutations and Factorials in C#"
date:   2009-10-23 00:00:05
categories: Programming
tags: [c-sharp, tutorials, maths]
---

Factorials are defined as a number that is multiplied by every positive number below it, and is denoted by an exclamation mark, ie _n! = n * (n – 1) * (n – 2) … 2 * 1_.

This is a very easy function to write.

{% highlight csharp %}
public static long Factorial(int num)  
{  
    long fact = 1;  
    for (int i = 2; i <= num; i++)  
    {  
        fact = fact * i;  
    }  
    return fact;  
}
{% endhighlight %}
<!--more-->
  
Combinations in Combinatorics (a branch of mathematics covering combinations and permutations) gives you the number of combinations that exist for a given sample. With combinations, items selected cannot be repeated, much like selecting names out of a hat, and then after taking one out, you do not put it back in again, and then select again until you have required amount.

So, given a set of values of size n, if you select r items from the set, the number of different combinations which you can select is given by the formula
_C = n! / r!(n - r)!_

Here is the code to calculate this. The MathExt class is just the name of the class containing these functions, so you can replace that with wherever you have defined the factorial function.

{% highlight csharp %}
public static int Combination(int n, int r)  
{  
    int Comb = 0;  
    Comb = (int)(MathExt.Factorial(n) / (MathExt.Factorial(r) * MathExt.Factorial(n - r)));  
    return Comb;  
} 
{% endhighlight %}

Permutations are a little simpler than combinations. In this case, you still selecting r items out of a set of n items, however, you are able to pull out the same item more than once. Much like pulling a name out of a hat, and then putting it back in before putting another name out of the hat.
_C = n! / (n - r)!_

{% highlight csharp %}
public static int Permutation(int n, int r)  
{  
    int Perm = 0;  
    Perm = (int)(MathExt.Factorial(n) / MathExt.Factorial(n - r));  
    return Perm;  
}  
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 23 Oct 2009_