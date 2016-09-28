---
layout: post
title:  "Image Processing in C#: Part 2 - Converting an Image to Greyscale"
date:   2010-01-19 00:00:02
categories: Programming
tags: [c-sharp, tutorials, image-processing, programming]
---

Converting an image to greyscale is a relatively easy effect to apply to an image. For each pixel in the image, we take the three colour components (red, green and blue), and then combine the values.

Most people would think that you just take the average of the three colours, but the problem with that is the human eye has different sensitivities to different frequencies, so what we need to do is add in the colours at different ratios.

So, to make the colour ratios correct, we multiply the red, green and blue components by 0.299, 0.587 and 0.114 respectively. We then assign the value to all the components of the pixel.
<!--more-->

![Greyscale image](/assets/images/blog/Garfield-Greyscale.jpg){: .shadow-image .centered }

{% highlight csharp %}
public void ApplyGreyscale()
{
    byte A, R, G, B;
    Color pixelColor;

    for (int y = 0; y < bitmapImage.Height; y++)
    {
        for (int x = 0; x < bitmapImage.Width; x++)
        {
            pixelColor = bitmapImage.GetPixel(x, y);
            A = pixelColor.A;
            R =  (byte)((0.299 * pixelColor.R) + (0.587 * pixelColor.G) + (0.114 * pixelColor.B));
            G = B = R;

            bitmapImage.SetPixel(x, y, Color.FromArgb((int)A, (int)R, (int)G, (int)B));
        }
    }

}
{% endhighlight %}

The full source used in the series is available from [https://github.com/sjmeunier/image-processor](https://github.com/sjmeunier/image-processor).

_Originally posted on my old blog, Smoky Cogs, on 19 Nov 2009_