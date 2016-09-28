---
layout: post
title:  "Finding the Midpoint Between Two GPS Coordinates"
date:   2009-10-08 00:00:03
categories: Programming
tags: [c-sharp, tutorials, geodesy, maths, programming]
---

Previously I showed how to [find the distance between coordinates](/programming/2009/10/08/finding-the-distance-between-two-gps-coordinates) and the [bearing](/programming/2009/10/08/finding-the-bearibg-between-two-gps-coordinates), but what if you want to find the midpoint of two coordinates.

The code in C&#35; is rather straightforward &#8211; that is, if you know a little bit of trigonometry.
<!--more-->

{% highlight csharp %}
namespace Geodesy
{
    public class Geodesy
    {
        public static double RadToDeg(double radians)
        {
            return radians * (180 / Math.PI);
        }

        public static double DegToRad(double degrees)
        {
            return degrees * (Math.PI / 180);
        }

        public static void MidpointCoords(double lat1, double long1, double lat2, double long2, ref double midpointLat, ref double midpointLong)
        {
            //Convert input values to radians
            lat1 = Geodesy.DegToRad(lat1);
            long1 = Geodesy.DegToRad(long1);
            lat2 = Geodesy.DegToRad(lat2);
            long2 = Geodesy.DegToRad(long2);
            double deltaLat = lat2 - lat1;
            double deltaLong = long2 - long1;

            double Bx = Math.Cos(lat2) * Math.Cos(deltaLong);
            double By = Math.Cos(lat2) * Math.Sin(deltaLong);
            midpointLat = Geodesy.RadToDeg(Math.Atan2(Math.Sin(lat1) + Math.Sin(lat2),
               Math.Sqrt((Math.Cos(lat1) + Bx) * (Math.Cos(lat1) + Bx) + By * By)));
            midpointLong = Geodesy.RadToDeg(long1 + Math.Atan2(By, Math.Cos(lat1) + Bx));

        }

    }
}
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 8 Oct 2009_
