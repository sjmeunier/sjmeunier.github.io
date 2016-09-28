---
layout: post
title:  "Plasmatic - My Second Android App"
date:   2011-03-02 00:00:00
categories: Programming
tags: [android, java, fractals, programming]
---

A few days ago I blogged about my [first Android app](/programming/2011/02/24/my-first-android-app-a-dutch-public-holiday-app.html), and in that post I promised another one coming along soon. Well, it is now done.

For my second foray into Android application development, I decided to port some C&#35; code I had previously written to draw a plasma fractal to Java.

![Plasma fractal](/assets/images/blog/fractals/plasma.jpg){: .shadow-image .centered }

The original code is based on a [Java applet written by Justin Seyster](http://www.ic.sunysb.edu/Stu/jseyster/plasma/), which I ported to C&#35;, and modified slightly, a few years ago for another project.

I have [previously blogged](/programming/2009/09/30/fractals-in-csharp-plasma-fractals.html) about the algorithm used to generate the plasma fractal itself in my series on Fractals in c&#35;, so I won't go into detail about how the fractal is generated here.

The application consists of three activies.
* The main activity displays the generated plasma fractal, with menu options to refresh the fractal, which regenerates the fractal, and saving the image to the sdcard. The menu also has options to launch the settings activity and the about activity.
* The settings activity enables you to change the fractal type - the options being, Plasma, Cloud and Grayscale &#8211; as well as the roughness, which changes the amount of variation in the image.
* The about activity merely showssome information about the application.

While, by no means, a complicated application, this app implements a fair amount of Androids basic features.

The source for the project is available at [https://github.com/sjmeunier/plasmatic](https://github.com/sjmeunier/plasmatic)

_Originally posted on my old blog, Smoky Cogs, on 3 Mar 2011_
