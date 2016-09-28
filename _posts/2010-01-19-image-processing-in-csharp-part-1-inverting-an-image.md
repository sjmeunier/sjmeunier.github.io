---
layout: post
title:  "Image Processing in C#: Part 1 - Inverting an image"
date:   2010-01-19 00:00:01
categories: Programming
tags: [c-sharp, tutorials, image-processing, programming]
---

This is the first part of my series on image processing, and to start off with, I will cover a relatively easy topic - how to invert the colours of an image. The full source used in the series is available from [https://github.com/sjmeunier/image-processor](https://github.com/sjmeunier/image-processor).

Actually inverting the colours of an image – in essence, creating a negative of the image, is remarkably easy. All it entails is getting the colour of each pixel, which is broken down into red, green and blue and then subtracting each component from 255.

The Bitmap object we are using in C&#35; uses colour components in the range of 0-255, hence why we subtract the value from 255. There is also an Alpha component to a colour, but we ignore that and just reuse the original value.

Once we have adjusted the colour components, we set the pixel to the new colour.
<!--more-->

![Inverted image](/assets/images/blog/Garfield-Inverted.jpg){: .shadow-image .centered }

The code below does not include the instantiation of the bitmapData variable, since the function below forms part of a larger class (available in the full source). It is a standard Bitmap object.

{% highlight csharp %}
public void ApplyInvert()
{
    byte A, R, G, B;
    Color pixelColor;

    for (int y = 0; y < bitmapImage.Height; y++)
    {
        for (int x = 0; x < bitmapImage.Width; x++)
        {
            pixelColor = bitmapImage.GetPixel(x, y);
            A = pixelColor.A;
            R = (byte)(255 - pixelColor.R);
            G = (byte)(255 - pixelColor.G);
            B = (byte)(255 - pixelColor.B);
            bitmapImage.SetPixel(x, y, Color.FromArgb((int)A, (int)R, (int)G, (int)B));
        }
    }

}
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 19 Nov 2009_
