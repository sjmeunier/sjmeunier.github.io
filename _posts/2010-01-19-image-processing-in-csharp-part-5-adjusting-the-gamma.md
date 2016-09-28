---
layout: post
title:  "Image Processing in C#: Part 5 - Adjusting the Gamma"
date:   2010-01-19 00:00:05
categories: Programming
tags: [c-sharp, tutorials, image-processing, programming]
---

Gamma correction has a history based on the properties of CRT’s and was first noted in televisions.

The light intensity of a CRT is not directly proportional to the input voltage, but is rather proportional to the input voltage raised to a particular power. This power was known as gamma, and varied with CRT. The value was usually around 2.5.

This means that different CRT's (televisions and monitors before the flatscreen days) all showed the same images slightly differently, and gamma correction was implement to correct this.

A gamma filter works by creating an array of 256 values called a gamma ramp for each value of the red, blue and green components. The gamma value must be between 0.2 and 5.

The formula for calculating the gamma ramp is _255 * (i / 255)1/gamma + 0.5_.

If this value is greater than 255, then it is clamped to 255. It is possible to have a different gamma value for each of the 3 colour components.

Then for each pixel in the image, we can substitute the value in this array for the original value of that component at that pixel.
<!--more-->

![Gamma](/assets/images/blog/Garfield-Gamma.jpg){: .shadow-image .centered }

{% highlight csharp %}
public void ApplyGamma(double r, double g, double b)
{
	byte A, R, G, B;
	Color pixelColor;

	byte[] redGamma = new byte[256];
	byte[] greenGamma = new byte[256];
	byte[] blueGamma = new byte[256];

	for (int i = 0; i < 256; ++i)
	{
		redGamma[i] = (byte)Math.Min(255, (int)((255.0
			* Math.Pow(i / 255.0, 1.0 / r)) + 0.5));
		greenGamma[i] = (byte)Math.Min(255, (int)((255.0
			* Math.Pow(i / 255.0, 1.0 / g)) + 0.5));
		blueGamma[i] = (byte)Math.Min(255, (int)((255.0
			* Math.Pow(i / 255.0, 1.0 / b)) + 0.5));
	}

	for (int y = 0; y < bitmapImage.Height; y++)
	{
		for (int x = 0; x < bitmapImage.Width; x++)
		{
			pixelColor = bitmapImage.GetPixel(x, y);
			A = pixelColor.A;
			R = redGamma[pixelColor.R];
			G = greenGamma[pixelColor.G];
			B = blueGamma[pixelColor.B];
			bitmapImage.SetPixel(x, y, Color.FromArgb((int)A, (int)R, (int)G, (int)B));
		}
	}
}
{% endhighlight %}

The full source used in the series is available from [https://github.com/sjmeunier/image-processor](https://github.com/sjmeunier/image-processor).

_Originally posted on my old blog, Smoky Cogs, on 19 Nov 2009_
