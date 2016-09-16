---
layout: post
title:  "Image Processing in C#: Part 11 - Getting That Old-time Effect with Sepia"
date:   2010-01-19 00:00:11
categories: Programming
tags: [c-sharp, tutorials, image-processing]
---

Back in the old days, and here I am talking about the 19th century old days, it was very common to apply a sepia tinting to photographs. This was a chemical process, which preserved the photos for longer, although it was also used for its aesthetic appeal. These days, with digital photograpy, we don't need to preserve photos like in those early days, but it still does give a nice warm tone to a photo, and is great for making a modern photo appear old-fashioned.

Converting a photo to sepia is a relatively easy effect. For each pixel in the image, we first convert the pixel to greyscale. We then add a value to the green component, and double that value to the red component to give the sepia effect.

The depth of the sepia effect is determined by this value, which we pass here as a parameter. A value of 0 will give a standard greyscale image.
<!--more-->

![Sepia](/assets/images/blog/Garfield-Sepia.jpg){: .shadow-image .centered }

{% highlight csharp %}
public void ApplySepia(int depth)
{
    int A, R, G, B;
    Color pixelColor;

    for (int y = 0; y < bitmapImage.Height; y++)
    {
        for (int x = 0; x < bitmapImage.Width; x++)
        {
            pixelColor = bitmapImage.GetPixel(x, y);
            A = pixelColor.A;
            R = (int)((0.299 * pixelColor.R) + (0.587 * pixelColor.G) + (0.114 * pixelColor.B));
            G = B = R;

            R += (depth * 2);
            if (R > 255)
            {
                R = 255;
            }
            G += depth;
            if (G > 255)
            {
                G = 255;
            }

            bitmapImage.SetPixel(x, y, Color.FromArgb(A, R, G, B));
        }
    }
}
{% endhighlight %}

The full source used in the series is available from [https://github.com/sjmeunier/image-processor](https://github.com/sjmeunier/image-processor).

_Originally posted on my old blog, Smoky Cogs, on 19 Nov 2009_