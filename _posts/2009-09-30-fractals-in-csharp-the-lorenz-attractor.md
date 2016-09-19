---
layout: post
title:  "Fractals in C#: The Lorenz Attractor"
date:   2009-09-30 00:00:09
categories: Programming
tags: [c-sharp, tutorials, fractals]
---

The Lorenz attractor is a fractal based on the Lorenz oscillator, which is a 3-dimensional chaotic system.

![Lorenz Attractor](/assets/images/blog/fractals/lorenz.jpg){: .shadow-image .centered }

For a full explanation on the Lorentz attractor, you can read the [wikipedia article](http://en.wikipedia.org/wiki/Lorenz_attractor).

The central equations needed for the Lorenz oscillator are:
_dx/dt = σ(y - x)_
_dy/dt = x(ρ - z) - y_
_dz/dt = xy - βz_

_σ_ is the Prandtl number, and is usually set to 10.
_ρ_ is the Rayleigh number and can be varied.
_β_ is set to 8/3.

Now, drawing the Lorenz attractor in C&#35;, we are going to iterate a fixed number of times through these equations. In the code below, _ρ_ is set to 28, and the number of iterations to 8000.

The code is able to draw the attractor in two dimensions or three dimensions, and the first part of the code calculates the transformations we will use later to transform the coordinates of attractor (which are in three dimensions) to screen coordinates.

We then iterate through each iteration, taking the x, y and z from the previous iteration, and running them through the Lorens oscillator equations to get our first set of derivatives.

We then add these to the previous x,y and z values, and then run them through the equations again, getting our second set of derivatives, and then add these to the x,y and z values as before. We repeat this until we get four sets of derivatives.

Once we have that we then add them together multiply by a scaling factor, and and them to x, y and z respectively.

Now that we have the new coordinates, we can plot a point at that position, applying our transformations to convert the three dimensional coordinates to screen coordinates.
<!--more-->

{% highlight csharp %}
public void Generate(Graphics g, int iWidth, int iHeight, int iDim)
{
	double rho, sigma, beta;
	double iterations;
	double x,y,z,d0_x,d0_y,d0_z,d1_x,d1_y,d1_z,d2_x,d2_y,d2_z;
	double d3_x,d3_y,d3_z,xt,yt,zt,dt,dt2,x_angle,y_angle,z_angle;
	double sx,sy,sz,cx,cy,cz,temp_x,temp_y,old_y;
	int i, row,col, old_row, old_col;
	int color = 0;

	iterations = 8000;
	rho = 28;
	sigma = 10;
	beta = 8.0 / 3.0;

	x_angle = 45;
	y_angle = 0;
	z_angle = 90;
	x_angle = DegToRad(x_angle);
	sx = Math.Sin(x_angle);
	cx = Math.Cos(x_angle);
	y_angle = DegToRad(y_angle);
	sy = Math.Sin(y_angle);
	cy = Math.Cos(y_angle);
	z_angle = DegToRad(z_angle);
	sz = Math.Sin(z_angle);
	cz = Math.Cos(z_angle);


	x = 0;
	y = 1;
	z = 0;

	if (iDim == 3)
	{
		old_col = (int)Math.Round(y*9.0);
		old_row = (int)Math.Round(350.0 - 6.56*z);
		g.DrawLine(new Pen(oColor[0]),0,348,638,348);
		g.DrawLine(new Pen(oColor[0]),320,2,320,348);
		g.DrawLine(new Pen(oColor[0]),320,348,648,140);
	}
	else
	{
		old_col = (int)Math.Round(y*9.0+320.0);
		old_row = (int)Math.Round(350.0 - 6.56*z);
		g.DrawLine(new Pen(oColor[0]),0,348,639,348);
		g.DrawLine(new Pen(oColor[0]),320,2,320,348);
	}
	dt = 0.01;
	dt2 = dt / 2.0;
	for (i = 0; i <= iterations; i++)
	{
		d0_x = sigma*(y-x)*dt2;
		d0_y = (-x*z + rho*x - y)*dt2;
		d0_z = (x*y - beta*z)*dt2;
		xt = x + d0_x;
		yt = y + d0_y;
		zt = z + d0_z;
		d1_x = (sigma*(yt-xt))*dt2;
		d1_y = (-xt*zt + rho*xt - yt)*dt2;
		d1_z =(xt*yt - beta*zt)*dt2;
		xt = x + d1_x;
		yt = y + d1_y;
		zt = z + d1_z;
		d2_x = (sigma*(yt-xt))*dt;
		d2_y = (-xt*zt + rho*xt - yt)*dt;
		d2_z =(xt*yt - beta*zt)*dt;
		xt = x + d2_x;
		yt = y + d2_y;
		zt = z + d2_z;
		d3_x = (sigma*(yt - xt))*dt2;
		d3_y = (-xt*zt + rho*xt - yt)*dt2;
		d3_z = (xt*yt - beta*zt/3)*dt2;
		old_y = y;
		x = x + (d0_x + d1_x + d1_x + d2_x + d3_x) * 0.33333333;
		y = y + (d0_y + d1_y + d1_y + d2_y + d3_y) * 0.33333333;
		z = z +  (d0_z + d1_z + d1_z + d2_z + d3_z) * 0.33333333;

		if (iDim == 3)
		{
			temp_x = x*cx + y*cy + z*cz;
			temp_y = x*sx + y*sy + z*sz;
			col = (int)Math.Round(temp_x*8.0 + 320.0);
			row = (int)Math.Round(350.0 - temp_y*5.0);
		}
		else
		{
			col = (int)Math.Round(y*9.0+320.0);
			row = (int)Math.Round(350 - 6.56*z);
		}
		if (col < 320)
		{
			if (old_col >= 320)
			{
				color++;
				color = color % 16;
			}
		}
		if (col > 320) 
		{
			if (old_col <= 320) 
			{
				color++;
				color=color % 16;
			}
		}
		g.DrawLine(new Pen(oColor[color]),old_col,old_row,col,row);
		old_row = row;
		old_col = col;
	}
}
{% endhighlight %}

The full source code is available at [https://github.com/sjmeunier/fractalize](https://github.com/sjmeunier/fractalize).

_Originally posted on my old blog, Smoky Cogs, on 30 Sep 2009_
