---
layout: post
title:  "Image Processing in C#: Part 3 - Adjusting the Brightness"
date:   2010-01-19 00:00:03
categories: Programming
tags: [c-sharp, tutorials, image-processing]
---

To increase the brightness of an image, all you need to do is add a value to each component for each pixel in the image.

Increasing the brightness is not always reversible, because as the colour value tops out at 255, if the increased value goes above this value, it will be limited to that value. This makes any reversal of the process impossible. The opposite is true for negative values. Then if the value falls below 0, then the values are limited.
<!--more-->

![Brightness](/assets/images/blog/Garfield-Brightness.jpg){: .shadow-image .centered }

{% highlight csharp %}
public void ApplyBrightness(int brightness)
{
	int A, R, G, B;
	Color pixelColor;

	for (int y = 0; y < bitmapImage.Height; y++)
	{
		for (int x = 0; x < bitmapImage.Width; x++)
		{
			pixelColor = bitmapImage.GetPixel(x, y);
			A = pixelColor.A;
			R =  pixelColor.R + brightness;
			if (R > 255)
			{
				R = 255;
			}
			else if (R < 0)
			{
				R = 0;
			}

			G = pixelColor.G + brightness;
			if (G > 255)
			{
				G = 255;
			}
			else if (G < 0)
			{
				G = 0;
			}

			B = pixelColor.B + brightness;
			if (B > 255)
			{
				B = 255;
			}
			else if (B < 0)
			{
				B = 0;
			}

			bitmapImage.SetPixel(x, y, Color.FromArgb(A, R, G, B));
		}
	}

}
{% endhighlight %}

The full source used in the series is available from [https://github.com/sjmeunier/image-processor](https://github.com/sjmeunier/image-processor).

_Originally posted on my old blog, Smoky Cogs, on 19 Nov 2009_