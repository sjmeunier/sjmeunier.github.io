---
layout: post
title:  "Image Processing in C#: Part 4 - Contrast"
date:   2010-01-19 00:00:04
categories: Programming
tags: [c-sharp, tutorials, image-processing]
---

Contrast in a picture is a measure of how close the range of colours in a picture are. The closer the colours are together, the lower the contrast.

The value for the contrast ranges from -100 to 100, with positive numbers increasing the contrast, and negative numbers decreasing the contrast.

Now, we calculate the contrast value which we will use in the calculation with _((100 + contrast) / 100)_

Then, for each pixel in the image, we take each component of the pixel, and divide the value by 255 to get a value between 0 and 1. We then subtract 0.5, and any values which are negative will have their contrast decreased, while any positive values will have their contrast increased.

We then add back the 0.5, and convert the value back to a range of 0-255. After that we set the pixel colour again.
<!--more-->

![Contrast](/assets/images/blog/Garfield-Contrast.jpg){: .shadow-image .centered }

{% highlight csharp %}
public void ApplyContrast(double contrast)
{
    double A, R, G, B;

    Color pixelColor;

    contrast = (100.0 + contrast) / 100.0;
    contrast *= contrast;

    for (int y = 0; y < bitmapImage.Height; y++)
    {
        for (int x = 0; x < bitmapImage.Width; x++)
        {
            pixelColor = bitmapImage.GetPixel(x, y);
            A = pixelColor.A;

            R = pixelColor.R / 255.0;
            R -= 0.5;
            R *= contrast;
            R += 0.5;
            R *= 255;

            if (R > 255)
            {
                R = 255;
            }
            else if (R < 0)
            {
                R = 0;
            }

            G = pixelColor.G / 255.0;
            G -= 0.5;
            G *= contrast;
            G += 0.5;
            G *= 255;
            if (G > 255)
            {
                G = 255;
            }
            else if (G < 0)
            {
                G = 0;
            }

            B = pixelColor.B / 255.0;
            B -= 0.5;
            B *= contrast;
            B += 0.5;
            B *= 255;
            if (B > 255)
            {
                B = 255;
            }
            else if (B < 0)
            {
                B = 0;
            }

            bitmapImage.SetPixel(x, y, Color.FromArgb((int)A, (int)R, (int)G, (int)B));
        }
    }
}
{% endhighlight %}

The full source used in the series is available from [https://github.com/sjmeunier/image-processor](https://github.com/sjmeunier/image-processor).

_Originally posted on my old blog, Smoky Cogs, on 19 Nov 2009_