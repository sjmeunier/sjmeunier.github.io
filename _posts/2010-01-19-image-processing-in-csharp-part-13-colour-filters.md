---
layout: post
title:  "Image Processing in C#: Part 13 - Colour Filters"
date:   2010-01-19 00:00:13
categories: Programming
tags: [c-sharp, tutorials, image-processing]
---

Implementing a colour filter is a rather simple affair. All a colour filter will do is specify the percentage of red, green and blue to leave in the image. So, for example, if you would like to see only the green component, then you keep the green component of each pixel unchanged, and set the blue and red components to 0.

You can create interesting filters by playing with the ratios too. So for example to get a brownish filter, you can set the red percentage multiplier to 1, green to 0.6 and blue to 0. Then multiply each component by the respective values. In this example, the red value will be unchanged, and blue will be removed entirely. The green component, however, will be set to 60% of the original value.
<!--more-->

![Red colour filter](/assets/images/blog/Garfield-ColorFilterRed.jpg){: .shadow-image .centered }

{% highlight csharp %}
public void ApplyColorFilter(double r, double g, double b)
{
    byte A, R, G, B;
    Color pixelColor;

    for (int y = 0; y < bitmapImage.Height; y++)
    {
        for (int x = 0; x < bitmapImage.Width; x++)
        {
            pixelColor = bitmapImage.GetPixel(x, y);
            A = pixelColor.A;
            R = (byte)(pixelColor.R * r);
            G = (byte)(pixelColor.G * g);
            B = (byte)(pixelColor.B * b);
            bitmapImage.SetPixel(x, y, Color.FromArgb((int)A, (int)R, (int)G, (int)B));
        }
    }
}
{% endhighlight %}

The full source used in the series is available from [https://github.com/sjmeunier/image-processor](https://github.com/sjmeunier/image-processor).

_Originally posted on my old blog, Smoky Cogs, on 19 Nov 2009_