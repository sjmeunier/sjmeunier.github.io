---
layout: post
title:  "Image Processing in C#: Part 9 - Applying a Mean Removal"
date:   2010-01-19 00:00:09
categories: Programming
tags: [c-sharp, tutorials, image-processing, programming]
---

Another type of sharpening effect is mean removal. It works similarly to the [standard sharpening](/programming/2010/01/19/image-processing-in-csharp-part-8-sharpening-the-image.html), except it subtracts values from all the surrounding pixels evenly.

The grid which we used for this effect is as follows
<table border='0'>
	<tr><td>-1</td><td>-1</td><td>-1</td></tr>
	<tr><td>-1</td><td>11</td><td>-1</td></tr>
	<tr><td>-1</td><td>-1</td><td>-1</td></tr>
</table>

This gives a much more noticeable sharpening effect on the image.
<!--more-->

![Mean removal](/assets/images/blog/Garfield-MeanRemoval.jpg){: .shadow-image .centered }

{% highlight csharp %}
public void ApplyMeanRemoval(double weight)
{
    ConvolutionMatrix matrix = new ConvolutionMatrix(3);
    matrix.SetAll(1);
    matrix.Matrix[0, 0] = -1;
    matrix.Matrix[1, 0] = -1;
    matrix.Matrix[2, 0] = -1;
    matrix.Matrix[0, 1] = -1;
    matrix.Matrix[1, 1] = weight;
    matrix.Matrix[2, 1] = -1;
    matrix.Matrix[0, 2] = -1;
    matrix.Matrix[1, 2] = -1;
    matrix.Matrix[2, 2] = -1;
    matrix.Factor = weight - 8;
    bitmapImage = Convolution3x3(bitmapImage, matrix);

}
{% endhighlight %}

The full source used in the series is available from [https://github.com/sjmeunier/image-processor](https://github.com/sjmeunier/image-processor).

_Originally posted on my old blog, Smoky Cogs, on 19 Nov 2009_
