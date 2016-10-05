---
layout: post
title:  "Maths Algorithms in C#: Generating a Random Normal Distribution Set"
date:   2016-10-05 20:00:00
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

For doing statistical analysis, it is often useful to have good random sample data. The method below is able to generate a dataset of randomised numbers which fit into a normal distribution. 

The normal distribution, also known as a bell curve, due to its shape, has the property that 68.27 percent of the values within the distribution fall within one standard deviation from the mean, and 95.45 percent of values fall within two standard deviations, all centred on the mean. 

With the parameters of the method, **samples** specifies the number of values to generate. **Mean** and **standardDeviation** specify the parameters of the normal distribution we want, and finally, **accuracy** specifies how closely we want ot match the curve.

The value for accuracy can be any integer greater than 1. The higher the number the better the results, but takes longer to process. I have found good results in the 1000-10000 range in testing, while even going as low as 1 gives acceptable results in osme cases.

So how does the code work?

C&#35;'s Random class generates a double between 0.0 and 1.0, which we then add together a number of these randomly generated numbers specified by the accuracy. The higher accuracy we want, the more random numbers we add together. 

This summed random number now can range from 0 up to _accuracy_, so we convert it to the range _-(accuracy / 2.0)_ to _+(accuracy / 2.0)_. 

Now, we normalising this number by multiply it by the standard deviation and the root of _(12.0 / accuracy)_. 

This process is repeated for each sample we want to generate, with the generated values all falling within the expected curve.

{% highlight csharp %}
public static List<double> GenerateRandomNormalDistribution(int samples, double mean, double standardDeviation, int accuracy)
{
	List<double> values = new List<double>();
	Random random = new Random();

	for(int i = 0; i < samples; i++)
	{
		double num = 0;
		for(int j = 0; j < accuracy; j++)
			num += random.NextDouble();
		values.Add(standardDeviation * Math.Sqrt(12.0 / accuracy) * (num - (accuracy / 2.0)) + mean);
	}

	return values;
}
{% endhighlight %}

The full sourcecode for the MathLib library is available at [https://github.com/sjmeunier/mathlib](https://github.com/sjmeunier/mathlib)