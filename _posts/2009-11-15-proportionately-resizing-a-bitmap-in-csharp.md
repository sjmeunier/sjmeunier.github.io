---
layout: post
title:  "Proportionately Resizing A Bitmap in C&#35;"
date:   2009-11-15 00:00:00
categories: Programming
tags: [c-sharp, tutorials]
---

Resizing a bitmap and keeping the sides proportions intact - in other words keeping the same aspect ratio, we need to specify a maximum width and maximum height.

The first thing we do is get the longest and shortest dimensions and divide the one by the other. We then have the ratio of the two sides which we will use further on in the calculations. 

Then, finding the new width and height, we set the new width to the maximum width, and the new height to the maximum height divided by the ratio of the sides. If however, the height was the largest side, we then set the new height to the maximum height, and the width to maximum width divided by the ratio of the sides.

Once we have done this, we can then create a new Bitmap from the old Bitmap, using the new width and height we have calculated and then return the Bitmap as the last thing we do.
<!--more-->

{% highlight csharp %}
public static Bitmap ProportionallyResizeBitmap(Bitmap sourceBitmap, int maxWidth, int maxHeight)
{
	// original dimensions
	int width = sourceBitmap.Width;
	int height = sourceBitmap.Height;

	// Find the longest and shortest dimentions
	int longestDimension = (width > height) ? width : height;
	int shortestDimension = (width < height) ? width : height;

	double factor = ((double)longestDimension) / (double)shortestDimension;

	// Set width as max
	double newWidth = maxWidth;
	double newHeight = maxWidth / factor;

	//If height is actually greater, then we reset it to use height instead of width
	if (width < height)
	{
		newWidth = maxHeight / factor;
		newHeight = maxHeight;
	}

	// Create new Bitmap at new dimensions based on original bitmap
	Bitmap resizedBitmap = new Bitmap((int)newWidth, (int)newHeight);
	using (Graphics g = Graphics.FromImage((System.Drawing.Image)resizedBitmap))
		g.DrawImage(sourceBitmap, 0, 0, (int)newWidth, (int)newHeight);
	return resizedBitmap;
}
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 15 Nov 2009_