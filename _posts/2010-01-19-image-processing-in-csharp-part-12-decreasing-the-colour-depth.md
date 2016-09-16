---
layout: post
title:  "Image Processing in C#: Part 12 - Decreasing the Colour Depth"
date:   2010-01-19 00:00:12
categories: Programming
tags: [c-sharp, tutorials, image-processing]
---

Decreasing the colour depth involves converting colour values to standard values.

Specifying an offset, preferably a value evenly divisible into 256, such as 16, 24 or 32, we then need to make sure that the red, green and blue components' values get rounded off to multiples of this offset.

For example, using an offset of 16, value colour values would be 0, 15, 31, 47,......, 255. and all values would need to be rounded off to these values for each of the colour components.

To be able to do this, we take the component value, and then add half of the offset to the value. We then subtract the modulo of this value and the offset. This then gives values along the lines of 0, 16, 32, ...., 256, so we will need to subtract 1 from this, but will need to put in a correction for 0, since that value needs to remain as it is.
<!--more-->

![Decrease colour depth](/assets/images/blog/Garfield-DecreaseColorDepth.jpg){: .shadow-image .centered }

{% highlight csharp %}
public void ApplyDecreaseColourDepth(int offset)
{
    int A, R, G, B;
    Color pixelColor;

    for (int y = 0; y < bitmapImage.Height; y++)
    {
        for (int x = 0; x < bitmapImage.Width; x++)
        {
            pixelColor = bitmapImage.GetPixel(x, y);
            A = pixelColor.A;
            R = ((pixelColor.R + (offset / 2)) - ((pixelColor.R + (offset / 2)) % offset) - 1);
            if (R < 0)
            {
                R = 0;
            }
            G = ((pixelColor.G + (offset / 2)) - ((pixelColor.G + (offset / 2)) % offset) - 1);
            if (G < 0)
            {
                G = 0;
            }
            B = ((pixelColor.B + (offset / 2)) - ((pixelColor.B + (offset / 2)) % offset) - 1);
            if (B < 0)
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