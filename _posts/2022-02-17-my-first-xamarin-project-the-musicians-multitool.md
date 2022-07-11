---
layout: post
title:  "My First Xamarin Project: The Musician's Multitool"
date:   2022-02-17 21:00:00
categories: Programming
tags: [xamarin, android, programming, music]
---

\[__UPDATE__: The Musician's Multitool has been rewritten from scratch. For more details see the [Pocket Music Theory post](https://sjmeunier.github.io/programming/2022/07/11/why-i-ditched-xamarin-for-java.html)\].

For a while now, I have been wanting to experiment a bit with using Xamarin as a platform for developing mobile applications. While .Net has been my primary dev environment for years, all the Android apps I have written so far have been in Java, and so was a bit apprehensive about changing that paradigm. 

So, deciding to take that plunge, now, after a number of months of slogging away during my evenings, and on the weekends, I have a new app that I am ready to share.

The Musician's Multitool is now available on the Google Play store. \[__UPDATE__: This app is now deprecated, and replaced with Pocket Music Theory on the [Google Play store](https://play.google.com/store/apps/details?id=com.magnaursa.pocketmusictheory). \]

So, what is this app all about then? 

The Musician's Multitool is a collection of tools useful for musicians, partly driven my own needs as an amateur musician. 

The following features are available in the app:
* _Interval, scale and chord finders_ - allow you to look up intervals, scales and chords, along with a music staff showing the relevant notes. These notes are also playable.
* _Modulations_ - provides a dictionary of common modulations between keys.
* _Musical instrument ranges and tunings_ - shows the ranges of various musical instruments and various common tunings for an array of string instruments
* _Terminology and notation dictionaries_ - are reference guide for reading and understanding music
* _Circle of Fifths_ - renders the relationships between major and minor keys.
* _Metronome_ - provides a fully customisable metronome.
* _Virtual Piano_ - covers the full range of a piano.
* _Chromatic Tuner_ - this tuner allows the adjustment of the reference pitch
* _Spectrum Analyzer_ - gives a view of the audio input as a waveform on an oscilloscope, as well as a frequency range graph.

![The Musician's Multitool](/assets/images/blog/musicians-multitool-screenshot.png){: .shadow-image .centered }

Writing the app in Xamarin was interesting, and the app allowed me to touch a significant portion of the various features of app development, from figuring out how to play audio, to styling lists well, to drawing custom graphics, and creating custom controls.

This was a fun adventure, and I am hoping to add even more to the app over time, but for now, I hope you enjoy.
