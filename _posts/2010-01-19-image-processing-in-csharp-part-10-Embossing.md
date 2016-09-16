---
layout: post
title:  "Image Processing in C#: Part 10 - Embossing"
date:   2010-01-19 00:00:10
categories: Programming
tags: [c-sharp, tutorials, image-processing]
---
One of the most striking effects to apply to an image is embossing. This is effect is a form of edge-detection, and is also done using convolution, which you can read more about on my post on [smoothing using convolution](/programming/2010/01/19/image-processing-in-csharp-part-6-smoothing-using-convolution.html).

The convolution grid we use in embossing is a little different to the others we have looked at so far. It has an offset of 127, and a fixed factor of 4. The offset makes the the image appear on average as a medium gray.

The grid we will use then is
<table border='0'>
	<tr><td>-1</td><td>0</td><td>-1</td></tr>
	<tr><td>0</td><td>4</td><td>0</td></tr>
	<tr><td>-1</td><td>0</td><td>-1</td></tr>
</table>
<!--more-->

![Emboss](/assets/images/blog/Garfield-Emboss.jpg){: .shadow-image .centered }

{% highlight csharp %}
public void ApplyEmboss(double weight)
{
    ConvolutionMatrix matrix = new ConvolutionMatrix(3);
    matrix.SetAll(1);
    matrix.Matrix[0, 0] = -1;
    matrix.Matrix[1, 0] = 0;
    matrix.Matrix[2, 0] = -1;
    matrix.Matrix[0, 1] = 0;
    matrix.Matrix[1, 1] = weight;
    matrix.Matrix[2, 1] = 0;
    matrix.Matrix[0, 2] = -1;
    matrix.Matrix[1, 2] = 0;
    matrix.Matrix[2, 2] = -1;
    matrix.Factor = 4;
    matrix.Offset = 127;
    bitmapImage = Convolution3x3(bitmapImage, matrix);

}
{% endhighlight %}

The full source used in the series is available from [https://github.com/sjmeunier/image-processor](https://github.com/sjmeunier/image-processor).

_Originally posted on my old blog, Smoky Cogs, on 19 Nov 2009_