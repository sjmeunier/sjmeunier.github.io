---
layout: post
title:  "Finding the Bearing Between Two GPS Coordinates"
date:   2009-10-08 00:00:02
categories: Programming
tags: [c-sharp, tutorials, geodesy, maths]
---

Don't you think it would be really great to be able to work out the precise bearing of a place in relation to where you are now? The calculation for this is rather simple if you have the geographical coordinates of the two points that you would like to find the bearing of.

The code below calculates the angle between the two coordinates. The last function call to **ConvertToBearing** puts the value into the right range. The atan function returns a value back (converted to degrees), which is in the range of -180 to 180 degrees, whereas, bearing is normally given in the range 0 - 360 degrees.
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

        public static double Bearing(double lat1, double long1, double lat2, double long2)
        {
            //Convert input values to radians
            lat1 = Geodesy.DegToRad(lat1);
            long1 = Geodesy.DegToRad(long1);
            lat2 = Geodesy.DegToRad(lat2);
            long2 = Geodesy.DegToRad(long2);

            double deltaLong = long2 - long1;

            double y = Math.Sin(deltaLong) * Math.Cos(lat2);
            double x = Math.Cos(lat1) * Math.Sin(lat2) -
                    Math.Sin(lat1) * Math.Cos(lat2) * Math.Cos(deltaLong);
            double bearing = Math.Atan2(y, x);
            return Geodesy.ConvertToBearing(Geodesy.RadToDeg(bearing));
        }

        public static double ConvertToBearing(double deg)
        {
            return (deg + 360) % 360;
        }
    }
}
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 8 Oct 2009_
