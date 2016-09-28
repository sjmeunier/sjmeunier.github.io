---
layout: post
title:  "Generating Various Audio Waveforms in C#"
date:   2009-10-05 00:00:00
categories: Programming
tags: [c-sharp, tutorials, audio, programming]
---

I did a little bit of tinkering with an audio application a while ago which generated a variety of different audio waveforms. It is surprisingly easy to generate quite a few of the more common ones such as white noise and sine waves, so here is a little howto to implement this in C&#35;.

White noise is pure and simply just random data being played back. It is the sound of static and of waterfalls. In practice, white noise is very useful for muffling other sounds, which is why you sometimes see in movies, scenes where to beat bugs listening in on conversations, the actors turn on a tap before talking about some state secret. The white noise from the flowing water defeats the listening devices. 

This is dreadfully easy to implement, and simply entails filling a buffer with random values between 0 and 255. The buffer can then be played back, or saved as an audio file.

{% highlight csharp %}
private void GenerateWhiteNoise(ref byte[] SampleBuffer, int NumSamples)
{
	Random rnd1 = new System.Random();

	for (int i = 0; i < NumSamples; i++)
	{
		SampleBuffer[i] = (byte) rnd1.Next(255);
	}
}
{% endhighlight %}

The next wave to look at is the sine wave. This type of wave gives a pure sounding tone. Here, what we need is the number of samples, the desired frequency and the sample rate.
<!--more-->

We first work out how many samples there are per wave, which is based on the sample rate and frequency, and then we work out how many radians there are per wave, which we then use in our sine function. We now loop through the number of samples, and calculate a sine value for that particular sample, and then set the buffer at that point to the sine value, with 127 (the midpoint) being the 0 point for the sine function, since the values of the buffer are from 0 to 255.

{% highlight csharp %}
private void GenerateSine(ref byte[] SampleBuffer, int NumSamples, double Frequency, double SampleRate)
{
	int SamplesPerWave = (int)(SampleRate / Frequency );
	double RadPerSample = (Math.PI * 2) / (double) SamplesPerWave;
	double SinVal = 0;

	for (int i = 0; i < NumSamples; i++)
	{
		SinVal = Math.Sin(RadPerSample * (double)(i % SamplesPerWave));
		SampleBuffer[i] = (byte) (Math.Floor(SinVal * 127) + 128);
	}
}
{% endhighlight %}

Square waves are also very common forms of waves, and are useful in electronics. As with the sine wave we need the number of samples, the frequency and the sample rate.

We work out the samples per wave as before, except we halve the total, and then the code is really easy.

We take a Value of 127, and then while looping through the number of samples, every time we have iterated through the number of samples in a wave, we multiply the value by -1.

We then set the buffer to the value + 127, which means that at every wave, the value of the sample alternates between 0 and 255, which generates a nice clean square wave.

{% highlight csharp %}
private void GenerateSquare(ref byte[] SampleBuffer, int NumSamples, double Frequency, double SampleRate)
{
	int SamplesPerWave = (int)(SampleRate / (Frequency * 2));

	int Value = 127;
	int Counter = 0;
	for (int i = 0; i < NumSamples; i++)
	{
		if (Counter >= SamplesPerWave)
		{
			Counter = 0;
			Value *= -1;
		}
		SampleBuffer[i] = (byte) (Value + 127);
		Counter++;
	}
}
{% endhighlight %}

The last wave we are going to look at is the sawtooth wave, which occurs in instruments such as the violin. It sounds a lot softer than the square wave, but still is quite grating compared to the sine wave. It is characterised by a triangular shaped wave with a steady increase in amplitude, and then a sharp drop at the end back to 0, then repeated for each wave.

As with the other waves, we have the number of samples, frequency and sample rate, and calculate the samples per wave.

We also have a delta value, which is the amount to increase the wave for each sample in the wave.

So looping through all the samples, if it is the start of a new wave, we set the value to 0, otherwise we increase it by the delta value up to a maximum of 255.

We then set the buffer to the value, and move on to the next sample.

{% highlight csharp %}
private void GenerateSawtooth(ref byte[] SampleBuffer, int NumSamples, double Frequency, double SampleRate)
{
	int SamplesPerWave = (int)(SampleRate / Frequency);

	int Value = 0;
	int Counter = 0;
	double Delta = 255.0 / (double)SamplesPerWave;
	for (int i = 0; i < NumSamples; i++)
	{
		if (Counter >= SamplesPerWave)
		{
			Counter = 0;
			Value = 0;
		}
		else
		{
			Value += (int)Math.Round(Delta, 0);
			if (Value > 255)
			{
				Value = 255;
			}
		}
		SampleBuffer[i] = (byte) (Value);
		Counter++;
	}
}
{% endhighlight %}

There are many other waveform types, which I might cover in another blog post, but these are your stock standard ones. Have fun playing around with them...

_Originally posted on my old blog, Smoky Cogs, on 5 Oct 2009_
