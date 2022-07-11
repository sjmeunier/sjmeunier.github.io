---
layout: post
title:  "Why I ditched Xamarin for Java - Re-releasing The Musician's Multitool as Pocket Music Theory"
date:   2022-07-11 21:00:00
categories: Programming
tags: [xamarin, android, programming, java, music]
---

Earlier this year I decided to experiment with writing a Xamarin app, out of which The Musician's Multitool was born. You can read about that [here](https://sjmeunier.github.io/programming/2022/02/17/my-first-xamarin-project-the-musicians-multitool.html).

It was fun to be able to code in C#, which is a language I am far more comfortable in than Java. However, the fact that it was not native Android development meant that the resulting app was incredibly slow, especially in loading, and the package size was very bloated.

Another thing I wanted to try, was to integrate low-latency audio packages in the project, using C++, in order to improve some of the audio latency issues I had in the Xamarin version, which would be much harder to do in Xamarin.

Due to these factors, primarily, I decided to rewrite the application in Java, making it a native Android app.

I have recreated pretty much the exact same look and feel of the app, as well as all of the previous functionality as before, but the app runs _much_ faster, and is more stable than before.

It actually takes far less time, at least for me, to create the same app in Java, as compared to Xamarin, so I think that moving forward, I have no reason to keep on using Xamarin.

I also renamed the app to Pocket Music Theory, which is also now the [Google Play store](https://play.google.com/store/apps/details?id=com.magnaursa.pocketmusictheory).

<!--more-->

As with the previous version, the following features are available in the app:
* _Interval, scale and chord finders_ - allow you to look up intervals, scales and chords, along with a music staff showing the relevant notes. These notes are also playable.
* _Modulations_ - provides a dictionary of common modulations between keys.
* _Musical instrument ranges and tunings_ - shows the ranges of various musical instruments and various common tunings for an array of string instruments
* _Terminology and notation dictionaries_ - are reference guide for reading and understanding music
* _Circle of Fifths_ - renders the relationships between major and minor keys.
* _Metronome_ - provides a fully customisable metronome.
* _Virtual Piano_ - covers the full range of a piano.
* _Chromatic Tuner_ - this tuner allows the adjustment of the reference pitch
* _Spectrum Analyzer_ - gives a view of the audio input as a waveform on an oscilloscope, as well as a frequency range graph.

![Pocket Music Theory](/assets/images/blog/pocket-music-theory.png){: .shadow-image .centered }


