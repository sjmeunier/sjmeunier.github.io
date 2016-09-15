---
layout: post
title:  "Image Processing in C#: Part 8 - Sharpening an Image"
date:   2010-01-19 00:00:00
categories: Programming
tags: C&#35; programming tutorials image-processing
---

Sharpening an image is merely the inverse of what we did to blur the image. It also uses the convolution function, which you can read in my post about [smoothing using convolution](2010/01/19/image-processing-in-csharp-part-6-smoothing-using-convolution.html).

Here we supply a convolution matrix set up as follows:
|0|-2|0|
|-2|11|-2|
|0|-2|0|

Here our weighting factor would total 3, since the negative numbers get subtracted from the weighting total. 
<!--more-->

![Sharpen](/assets/images/blog/Garfield-Sharpen.jpg){: .shadow-image .centered }

{% highlight csharp %}
public void ApplySharpen(double weight)
{
    ConvolutionMatrix matrix = new ConvolutionMatrix(3);
    matrix.SetAll(1);
    matrix.Matrix[0, 0] = 0;
    matrix.Matrix[1, 0] = -2;
    matrix.Matrix[2, 0] = 0;
    matrix.Matrix[0, 1] = -2;
    matrix.Matrix[1, 1] = weight;
    matrix.Matrix[2, 1] = -2;
    matrix.Matrix[0, 2] = 0;
    matrix.Matrix[1, 2] = -2;
    matrix.Matrix[2, 2] = 0;
    matrix.Factor = weight - 8;
    bitmapImage = Convolution3x3(bitmapImage, matrix);

}
{% endhighlight %}

The full source used in the series is available from [https://github.com/sjmeunier/image-processor](https://github.com/sjmeunier/image-processor).

_Originally posted on my old blog, Centre of the Universe, on 19 Nov 2009_