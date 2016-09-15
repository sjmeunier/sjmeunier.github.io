---
layout: post
title:  "Image Processing in C#: Part 7 - Applying a Gaussian Blur"
date:   2010-01-19 00:00:00
categories: Programming
tags: C&#35; programming tutorials image-processing
---

Gaussian blur also uses convolution to create the effect. If you have not yet read my blog post on [smoothing using convolution](2010/01/19/image-processing-in-csharp-part-6-smoothing-using-convolution.html), I would advise you to read that post first, as in that post I explain how the convolution function works.

Gaussian blur works very similarly to the smoothing function, in that both are a type of blur, but the Gaussian blur gives a more natural looking blur. It achieves this by not have a flat weighting, but instead has a weighting based on a Gaussian curve, thus for our 3x3 grid, the values we want to supply the convolution function is as follows:
<!--more-->

|1|2|1|
|2|4|2|
|1|2|1|

The factor parameter is set to the sum of each pixel in the grid, which in this case, is again, 16, and the offset is set to 0.

Now we will get a nice blur effect.

![Gaussian blur](/assets/images/blog/Garfield-Gaussian.jpg){: .shadow-image .centered }

{% highlight csharp %}
public void ApplyGaussianBlur(double peakValue)
{
    ConvolutionMatrix matrix = new ConvolutionMatrix(3);
    matrix.SetAll(1);
    matrix.Matrix[0, 0] = peakValue / 4;
    matrix.Matrix[1, 0] = peakValue / 2;
    matrix.Matrix[2, 0] = peakValue / 4;
    matrix.Matrix[0, 1] = peakValue / 2;
    matrix.Matrix[1, 1] = peakValue;
    matrix.Matrix[2, 1] = peakValue / 2;
    matrix.Matrix[0, 2] = peakValue / 4;
    matrix.Matrix[1, 2] = peakValue / 2;
    matrix.Matrix[2, 2] = peakValue / 4;
    matrix.Factor = peakValue * 4;
    bitmapImage = Convolution3x3(bitmapImage, matrix);

}
{% endhighlight %}

The full source used in the series is available from [https://github.com/sjmeunier/image-processor](https://github.com/sjmeunier/image-processor).

_Originally posted on my old blog, Centre of the Universe, on 19 Nov 2009_