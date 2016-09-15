---
layout: post
title:  "Image Processing in C#: Part 6 - Smoothing Usingg Convolution"
date:   2010-01-19 00:00:00
categories: Programming
tags: C&#35; programming tutorials image-processing
---

Smoothing, which is just a type of blurring is based on a concept called convolution. How this works is that you take a square section of pixels, often 3x3 but can be any size, although the larger you make the grid, the less pixels that will get affected by the effect around the edges.

The centre pixel of the grid is the main pixel we are interested in. To use convolution, what we do is assign weightings to each pixel in the grid, depending on what effect we are trying to achieve, and then assign the result to the centre pixel.

The basic data structure we use for the convolution is a class which contains the 2D array, as well as a weighting factor, and an offset.
<!--more-->

{% highlight csharp %}
    public class ConvolutionMatrix
    {
        public int MatrixSize = 3;

        public double[,] Matrix;
        public double Factor = 1;
        public double Offset = 1;

        public ConvolutionMatrix(int size)
        {
            MatrixSize = 3;
            Matrix = new double[size, size];
        }

        public void SetAll(double value)
        {
            for (int i = 0; i < MatrixSize; i++)
            {
                for (int j = 0; j < MatrixSize; j++)
                {
                    Matrix[i, j] = value;
                }
            }
        }
    }
{% endhighlight %}

Now, having this class, we can implement the convolution algorithm

{% highlight csharp %}
public Bitmap Convolution3x3(Bitmap b, ConvolutionMatrix m)
{
    Bitmap newImg = (Bitmap)b.Clone();
    Color[,] pixelColor = new Color[3, 3];
    int A, R, G, B;

    for (int y = 0; y < b.Height - 2; y++)
    {
        for (int x = 0; x < b.Width - 2; x++)
        {
            pixelColor[0, 0] = b.GetPixel(x, y);
            pixelColor[0, 1] = b.GetPixel(x, y + 1);
            pixelColor[0, 2] = b.GetPixel(x, y + 2);
            pixelColor[1, 0] = b.GetPixel(x + 1, y);
            pixelColor[1, 1] = b.GetPixel(x + 1, y + 1);
            pixelColor[1, 2] = b.GetPixel(x + 1, y + 2);
            pixelColor[2, 0] = b.GetPixel(x + 2, y);
            pixelColor[2, 1] = b.GetPixel(x + 2, y + 1);
            pixelColor[2, 2] = b.GetPixel(x + 2, y + 2);

            A = pixelColor[1, 1].A;

            R = (int)((((pixelColor[0, 0].R * m.Matrix[0, 0]) +
                         (pixelColor[1, 0].R * m.Matrix[1, 0]) +
                         (pixelColor[2, 0].R * m.Matrix[2, 0]) +
                         (pixelColor[0, 1].R * m.Matrix[0, 1]) +
                         (pixelColor[1, 1].R * m.Matrix[1, 1]) +
                         (pixelColor[2, 1].R * m.Matrix[2, 1]) +
                         (pixelColor[0, 2].R * m.Matrix[0, 2]) +
                         (pixelColor[1, 2].R * m.Matrix[1, 2]) +
                         (pixelColor[2, 2].R * m.Matrix[2, 2]))
                                / m.Factor) + m.Offset);

            if (R < 0)
            {
                R = 0;
            }
            else if (R > 255)
            {
                R = 255;
            }

            G = (int)((((pixelColor[0, 0].G * m.Matrix[0, 0]) +
                         (pixelColor[1, 0].G * m.Matrix[1, 0]) +
                         (pixelColor[2, 0].G * m.Matrix[2, 0]) +
                         (pixelColor[0, 1].G * m.Matrix[0, 1]) +
                         (pixelColor[1, 1].G * m.Matrix[1, 1]) +
                         (pixelColor[2, 1].G * m.Matrix[2, 1]) +
                         (pixelColor[0, 2].G * m.Matrix[0, 2]) +
                         (pixelColor[1, 2].G * m.Matrix[1, 2]) +
                         (pixelColor[2, 2].G * m.Matrix[2, 2]))
                                / m.Factor) + m.Offset);

            if (G < 0)
            {
                G = 0;
            }
            else if (G > 255)
            {
                G = 255;
            }

            B = (int)((((pixelColor[0, 0].B * m.Matrix[0, 0]) +
                         (pixelColor[1, 0].B * m.Matrix[1, 0]) +
                         (pixelColor[2, 0].B * m.Matrix[2, 0]) +
                         (pixelColor[0, 1].B * m.Matrix[0, 1]) +
                         (pixelColor[1, 1].B * m.Matrix[1, 1]) +
                         (pixelColor[2, 1].B * m.Matrix[2, 1]) +
                         (pixelColor[0, 2].B * m.Matrix[0, 2]) +
                         (pixelColor[1, 2].B * m.Matrix[1, 2]) +
                         (pixelColor[2, 2].B * m.Matrix[2, 2]))
                                / m.Factor) + m.Offset);

            if (B < 0)
            {
                B = 0;
            }
            else if (B > 255)
            {
                B = 255;
            }
            newImg.SetPixel(x+1, y+1, Color.FromArgb(A, R, G, B));
        }
    }
    return newImg;
}
{% endhighlight %}

Finally, after all that, we are ready to implement a smoothing function. To smooth an image, we assign a particular value to the centre pixel, and then add 1 to all the surrounding pixels, so the array would look something like this

|1|1|1|
|1|8|1|
|1|1|1|

The factor parameter is set to the sum of each pixel in the grid, which in this case would be 16, and the offset is set to 0. The offset shifts all the values of the colour components by the specified amount.

Now all that is left is calling the convolution function.

![Smooth](/assets/images/blog/Garfield-Smooth.jpg){: .shadow-image .centered }

{% highlight csharp %}
public void ApplySmooth(double weight)
{
	ConvolutionMatrix matrix = new ConvolutionMatrix(3);
	matrix.SetAll(1);
	matrix.Matrix[1,1] = weight;
	matrix.Factor = weight + 8;
	bitmapImage = Convolution3x3(bitmapImage, matrix);

}
{% endhighlight %}

The full source used in the series is available from [https://github.com/sjmeunier/image-processor](https://github.com/sjmeunier/image-processor).

_Originally posted on my old blog, Centre of the Universe, on 19 Nov 2009_
