---
layout: post
title:  "Astro Finder Chart"
date:   2016-09-19 21:30:00
categories: Programming
tags: [c-sharp, astronomy]
---

Good old fashioned finder charts have become a bit less frequently used these days when it is easy to control your telescope from one of the many planetarium and charting software available, but sometimes it is useful to have a proper finder chart at the telescope.

I have ported [fchart](https://github.com/Fingel/fchart) by Austin Riba, written in Python, which he forked from the [original project by Michiel Brentjen](https://www.astro.rug.nl/~brentjen/fchart.html), to C&#35;, and extended it slightly.

The application allows you to generate finder charts for any part of the sky, and includes the full NGC and IC catalogues with quite a number of deep sky objects more. The app allows searching for objects, and will centre the coordinates on that object, or coordinates can be entered manually. 
<!--more-->

The resolution of the image, field of view, and configuration options on what to show can also be configured before generating an image. Once generated the image can be saved as a PNG file.

The full source of the project is available on [GitHub](https://github.com/sjmeunier/astro-finder-chart)

[![Chart generated for M31](/assets/images/blog/astro-finder-chart-m31-small.png){: .shadow-image .centered }](/assets/images/blog/astro-finder-chart-m31.png)
_Click on image to load full image_