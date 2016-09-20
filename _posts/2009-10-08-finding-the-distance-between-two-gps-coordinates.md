---
layout: post
title:  "Finding the Distance Between Two GPS Coordinates"
date:   2009-10-08 00:00:01
categories: Programming
tags: [c-sharp, tutorials, geodesy, maths]
---

With the advent of GPS systems, adn mapping systems like Google Earth, it has become rather useful to be able to calculate the distance (as the crow flies) between two points on the earth.

One method to do so is known as the Haversine formula.

Taking the longitude and latitude of two points, and then converting them to radians, since the C&#35; trig functions take values in radians, not in degrees.

We then take the difference of latitude and longitude of the two points. Applying the Haversine formula needed and then multiplying by the radius of the earth gives us the distance in kilometres of the two points.

a = sin²(Δlat/2) + cos(lat1).cos(lat2).sin²(Δlong/2)
c = 2.atan2(√a, √(1−a))
Distance = (radius of earth) * c

This method of calculating the distance is not entirely accurate though, but will suffice for all but the most exacting of applications. For every kilometer, there is an average error of 3 meters, or roughly a 0.3-0.5% error. This error creeps in because the formula assumes a perfectly spherical earth, whereas the earth is slightly ellipsoid, so the radius around the equator is slightly bigger than the radius around the poles.
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


        //Calculate distance between two coordinates useing the Haversine formula
        public static double DistanceBetweenCoords(double lat1, double long1, double lat2, double long2)
        {
            //Convert input values to radians
            lat1 = Geodesy.DegToRad(lat1);
            long1 = Geodesy.DegToRad(long1);
            lat2 = Geodesy.DegToRad(lat2);
            long2 = Geodesy.DegToRad(long2);

            double earthRadius = 6371; // km
            double deltaLat = lat2 - lat1;
            double deltaLong = long2 - long1;
            double a = Math.Sin(deltaLat / 2.0) * Math.Sin(deltaLat / 2.0) +
                    Math.Cos(lat1) * Math.Cos(lat2) *
                    Math.Sin(deltaLong / 2.0) * Math.Sin(deltaLong / 2.0);
            double c = 2 * Math.Atan2(Math.Sqrt(a), Math.Sqrt(1 - a));
            double distance = earthRadius * c;

            return distance;
        }

}
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 8 Oct 2009_
