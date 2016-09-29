---
layout: post
title:  "A Microfont using Sub-Pixel Rendering"
date:   2016-09-29 20:30:00
categories: Programming
tags: [c-sharp, tutorials, graphics, programming]
---

A few years ago I came across a [blog post on Distractionware by Terry Cavanagh](http://distractionware.com/blog/2008/04/sub-pixel-message-generator/) explaining how he created a 1x5 pixel font using sub-pixel rendering.

Before explaining about the font, let's first talk about sub-pixel rendering. Sub-pixel rendering is the process of drawing on the screen by taking advantage of the fact that a single pixel on screen is actually made up of 3 separate coloured pixels.

In an LCD screen, these three sub-pixels, coloured red, green and blue, are laid out horizontally, repeating for each whole pixel on-screen, as indicated below.
<center>
<table style="border: 1px solid #000000;">
<tr><td style="width: 10px; color: #ff0000;">&nbsp;</td><td style="width: 10px; color: #00ff00;">&nbsp;</td><td style="width: 10px; color: #0000ff;">&nbsp;</td></tr>
<tr><td style="width: 10px; color: #ff0000;">&nbsp;</td><td style="width: 10px; color: #00ff00;">&nbsp;</td><td style="width: 10px; color: #0000ff;">&nbsp;</td></tr>
<tr><td style="width: 10px; color: #ff0000;">&nbsp;</td><td style="width: 10px; color: #00ff00;">&nbsp;</td><td style="width: 10px; color: #0000ff;">&nbsp;</td></tr>
<table>
</center>

By using one of these three individual colours we can then draw onscreen at resolutions higher than a whole pixel. This technique is used to create the 1x5 pixel font. Using the sub-pixel rendering, we are able to create a font which, in effect, is 3 pixels wide and 5 pixels high, while only using 1 physical pixel width.

The version of the font demonstrated on Distractionware was written in C++, so I borrowed the idea, and wrote a C&#35; version of the font using the same technique. Below we can see the result.

![Microfont app](/assets/images/blog/microfont-app.png){: .shadow-image .centered }

And by zooming in on the output
![Enlarged font](/assets/images/blog/microfont-enlarged.png){: .shadow-image .centered }

As you can see, the results are (just barely) readable. It is not the most practical of fonts, but the technique does produce results surprisingly more legible than you would expect.
<!--more-->

Now we can look at the code. The full source of my c&$35; implementation can be found at [https://github.com/sjmeunier/microfont](https://github.com/sjmeunier/microfont).

The first important class is the **FontDefinition** class which contains the array definition of each character in the font.
{% highlight csharp %}
using System.Collections.Generic;

namespace MicroFont
{
    public class FontDefinition
    {
        public Dictionary<char, int[,]> Characters;

        public FontDefinition()
        {
            Characters = new Dictionary<char, int[,]>();

            Characters.Add('A', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 1 }, { 1, 1, 1 }, { 1, 0, 1 }, { 1, 0, 1 } });
            Characters.Add('B', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 1 }, { 1, 1, 0 }, { 1, 0, 1 }, { 1, 1, 1 } });
            Characters.Add('C', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 0 }, { 1, 0, 0 }, { 1, 0, 0 }, { 1, 0, 1 } });
            Characters.Add('D', new int[5, 3] { { 1, 1, 0 }, { 1, 0, 1 }, { 1, 0, 1 }, { 1, 0, 1 }, { 1, 0, 0 } });
            Characters.Add('E', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 0 }, { 1, 1, 0 }, { 1, 0, 0 }, { 1, 0, 1 } });
            Characters.Add('F', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 0 }, { 1, 1, 0 }, { 1, 0, 0 }, { 1, 0, 0 } });
            Characters.Add('G', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 0 }, { 1, 0, 1 }, { 1, 0, 1 }, { 1, 1, 1 } });
            Characters.Add('H', new int[5, 3] { { 1, 0, 1 }, { 1, 0, 1 }, { 1, 1, 1 }, { 1, 0, 1 }, { 1, 0, 1 } });
            Characters.Add('I', new int[5, 3] { { 1, 1, 1 }, { 0, 1, 0 }, { 0, 1, 0 }, { 0, 1, 0 }, { 1, 1, 1 } });
            Characters.Add('J', new int[5, 3] { { 1, 1, 1 }, { 0, 1, 0 }, { 0, 1, 0 }, { 0, 1, 0 }, { 1, 1, 0 } });
            Characters.Add('K', new int[5, 3] { { 1, 0, 0 }, { 1, 0, 1 }, { 1, 1, 0 }, { 1, 0, 1 }, { 1, 0, 1 } });
            Characters.Add('L', new int[5, 3] { { 1, 0, 0 }, { 1, 0, 0 }, { 1, 0, 0 }, { 1, 0, 0 }, { 1, 1, 1 } });
            Characters.Add('M', new int[5, 3] { { 1, 0, 1 }, { 1, 1, 1 }, { 1, 1, 1 }, { 1, 0, 1 }, { 1, 0, 1 } });
            Characters.Add('N', new int[5, 3] { { 1, 0, 1 }, { 1, 1, 1 }, { 1, 1, 1 }, { 1, 1, 1 }, { 1, 0, 1 } });
            Characters.Add('O', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 1 }, { 1, 0, 1 }, { 1, 0, 1 }, { 1, 1, 1 } });
            Characters.Add('P', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 1 }, { 1, 1, 1 }, { 1, 0, 0 }, { 1, 0, 0 } });
            Characters.Add('Q', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 1 }, { 1, 0, 1 }, { 1, 1, 1 }, { 1, 1, 1 } });
            Characters.Add('R', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 1 }, { 1, 1, 0 }, { 1, 0, 1 }, { 1, 0, 1 } });
            Characters.Add('S', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 0 }, { 1, 1, 1 }, { 0, 0, 1 }, { 1, 1, 1 } });
            Characters.Add('T', new int[5, 3] { { 1, 1, 1 }, { 0, 1, 0 }, { 0, 1, 0 }, { 0, 1, 0 }, { 0, 1, 0 } });
            Characters.Add('U', new int[5, 3] { { 1, 0, 1 }, { 1, 0, 1 }, { 1, 0, 1 }, { 1, 0, 1 }, { 0, 1, 0 } });
            Characters.Add('V', new int[5, 3] { { 1, 0, 1 }, { 1, 0, 1 }, { 1, 0, 1 }, { 0, 1, 0 }, { 0, 1, 0 } });
            Characters.Add('W', new int[5, 3] { { 1, 0, 1 }, { 1, 0, 1 }, { 1, 1, 1 }, { 1, 1, 1 }, { 1, 0, 1 } });
            Characters.Add('X', new int[5, 3] { { 1, 0, 1 }, { 1, 0, 1 }, { 0, 1, 0 }, { 1, 0, 1 }, { 1, 0, 1 } });
            Characters.Add('Y', new int[5, 3] { { 1, 0, 1 }, { 1, 0, 1 }, { 1, 1, 1 }, { 0, 1, 0 }, { 0, 1, 0 } });
            Characters.Add('Z', new int[5, 3] { { 1, 1, 1 }, { 0, 0, 1 }, { 0, 1, 0 }, { 1, 0, 0 }, { 1, 1, 1 } });
            Characters.Add('1', new int[5, 3] { { 0, 1, 0 }, { 0, 1, 0 }, { 0, 1, 0 }, { 0, 1, 0 }, { 1, 1, 1 } });
            Characters.Add('2', new int[5, 3] { { 1, 1, 1 }, { 0, 0, 1 }, { 0, 1, 1 }, { 1, 1, 0 }, { 1, 1, 1 } });
            Characters.Add('3', new int[5, 3] { { 1, 1, 1 }, { 0, 0, 1 }, { 0, 1, 1 }, { 0, 0, 1 }, { 1, 1, 1 } });
            Characters.Add('4', new int[5, 3] { { 1, 0, 0 }, { 1, 0, 0 }, { 1, 1, 0 }, { 1, 1, 1 }, { 0, 1, 0 } });
            Characters.Add('5', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 0 }, { 1, 1, 0 }, { 0, 0, 1 }, { 1, 1, 0 } });
            Characters.Add('6', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 0 }, { 1, 1, 1 }, { 1, 0, 1 }, { 1, 1, 1 } });
            Characters.Add('7', new int[5, 3] { { 1, 1, 1 }, { 0, 0, 1 }, { 0, 0, 1 }, { 0, 0, 1 }, { 0, 0, 1 } });
            Characters.Add('8', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 1 }, { 1, 1, 1 }, { 1, 0, 1 }, { 1, 1, 1 } });
            Characters.Add('9', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 1 }, { 1, 1, 1 }, { 0, 0, 1 }, { 0, 0, 1 } });
            Characters.Add('0', new int[5, 3] { { 0, 1, 0 }, { 1, 0, 1 }, { 1, 0, 1 }, { 1, 0, 1 }, { 0, 1, 0 } });
            Characters.Add('!', new int[5, 3] { { 0, 1, 0 }, { 0, 1, 0 }, { 0, 1, 0 }, { 0, 0, 0 }, { 0, 1, 0 } });
            Characters.Add('\"', new int[5, 3] { { 1, 0, 1 }, { 1, 0, 1 }, { 0, 0, 0 }, { 0, 0, 0 }, { 0, 0, 0 } });
            Characters.Add('#', new int[5, 3] { { 1, 0, 1 }, { 1, 1, 1 }, { 1, 0, 1 }, { 1, 1, 1 }, { 1, 0, 1 } });
            Characters.Add('$', new int[5, 3] { { 1, 1, 1 }, { 1, 1, 0 }, { 1, 1, 1 }, { 0, 1, 1 }, { 1, 1, 1 } });
            Characters.Add('%', new int[5, 3] { { 1, 0, 1 }, { 0, 0, 1 }, { 0, 1, 0 }, { 1, 0, 0 }, { 1, 0, 1 } });
            Characters.Add('&', new int[5, 3] { { 0, 1, 0 }, { 1, 0, 1 }, { 1, 1, 1 }, { 1, 0, 1 }, { 1, 1, 1 } });
            Characters.Add('\'', new int[5, 3] { { 0, 1, 0 }, { 0, 1, 0 }, { 0, 0, 0 }, { 0, 0, 0 }, { 0, 0, 0 } });
            Characters.Add('(', new int[5, 3] { { 0, 1, 1 }, { 1, 0, 0 }, { 1, 0, 0 }, { 1, 0, 0 }, { 0, 1, 1 } });
            Characters.Add(')', new int[5, 3] { { 1, 1, 0 }, { 0, 0, 1 }, { 0, 0, 1 }, { 0, 0, 1 }, { 1, 1, 0 } });
            Characters.Add('*', new int[5, 3] { { 0, 0, 0 }, { 0, 1, 0 }, { 1, 1, 1 }, { 0, 1, 0 }, { 1, 0, 1 } });
            Characters.Add('+', new int[5, 3] { { 0, 0, 0 }, { 0, 1, 0 }, { 1, 1, 1 }, { 0, 1, 0 }, { 0, 0, 0 } });
            Characters.Add(',', new int[5, 3] { { 0, 0, 0 }, { 0, 0, 0 }, { 0, 0, 0 }, { 0, 1, 1 }, { 1, 1, 0 } });
            Characters.Add('.', new int[5, 3] { { 0, 0, 0 }, { 0, 0, 0 }, { 0, 0, 0 }, { 0, 1, 1 }, { 0, 1, 1 } });
            Characters.Add(':', new int[5, 3] { { 0, 0, 0 }, { 0, 1, 0 }, { 0, 0, 0 }, { 0, 1, 0 }, { 0, 0, 0 } });
            Characters.Add(';', new int[5, 3] { { 0, 0, 0 }, { 0, 1, 0 }, { 0, 0, 0 }, { 0, 1, 0 }, { 1, 0, 0 } });
            Characters.Add('<', new int[5, 3] { { 0, 0, 1 }, { 0, 1, 0 }, { 1, 0, 0 }, { 0, 1, 0 }, { 0, 0, 1 } });
            Characters.Add('>', new int[5, 3] { { 1, 0, 0 }, { 0, 1, 0 }, { 0, 0, 1 }, { 0, 1, 0 }, { 1, 0, 0 } });
            Characters.Add('?', new int[5, 3] { { 1, 1, 1 }, { 0, 0, 1 }, { 0, 1, 0 }, { 0, 0, 0 }, { 0, 1, 0 } });
            Characters.Add('@', new int[5, 3] { { 1, 1, 1 }, { 1, 0, 1 }, { 1, 0, 1 }, { 1, 1, 1 }, { 1, 1, 1 } });
            Characters.Add('=', new int[5, 3] { { 0, 0, 0 }, { 1, 1, 1 }, { 0, 0, 0 }, { 1, 1, 1 }, { 0, 0, 0 } });
            Characters.Add('[', new int[5, 3] { { 1, 1, 0 }, { 1, 0, 0 }, { 1, 0, 0 }, { 1, 0, 0 }, { 1, 1, 0 } });
            Characters.Add(']', new int[5, 3] { { 0, 1, 1 }, { 0, 0, 1 }, { 0, 0, 1 }, { 0, 0, 1 }, { 0, 1, 1 } });
            Characters.Add('-', new int[5, 3] { { 0, 0, 0 }, { 0, 0, 0 }, { 1, 1, 1 }, { 0, 0, 0 }, { 0, 0, 0 } });
            Characters.Add(' ', new int[5, 3] { { 0, 0, 0 }, { 0, 0, 0 }, { 0, 0, 0 }, { 0, 0, 0 }, { 0, 0, 0 } });
        }
    }
}
{% endhighlight %}

The main processing of the font happens in the **MicroFont** class. There are two main methods we need
* **MeasureText** takes the input text and measures the size in pixels of the output which would get generated by the renderer
* **Generate** which renders the input text in the 1x5 microfont. The text is drawn directly onto a Bitmap using the mapping in hte font definition. For each row in the character to be rendered, the red, green and blue channels are independantly set to create the font.
{% highlight csharp %}
using System;
using System.Drawing;

namespace MicroFont
{
    public class MicroFont
    {
        private FontDefinition font = new FontDefinition();

        public MicroFont()
        {

        }

		public Size MeasaureText(string text, int xPadding, int yPadding)
		{
			Size size = new Size(xPadding, yPadding);

			int sizeX = xPadding;

			foreach (char character in text.ToUpper())
			{
				if (character == '\n')
				{
					size.Height += 6;
					size.Width = Math.Max(size.Width, sizeX);
					sizeX = xPadding;
					continue;
				}
				if (font.Characters.ContainsKey(character))
				{
					int[,] fontChar = font.Characters[character];
					sizeX += 2;
				}

			}
			size.Width = Math.Max(size.Width, sizeX) + xPadding - 1;
			size.Height += 5 + yPadding;

			return size;
		}

		public Bitmap Generate(string text, int xPadding, int yPadding, bool invertColours)
		{
			Size size = MeasaureText(text, xPadding, yPadding);
			return Generate(text, size.Width, size.Height, xPadding, yPadding, invertColours);
		}

		public Bitmap Generate(string text, int width, int height, int xPadding, int yPadding, bool invertColours)
        {
            Bitmap image = new Bitmap(width, height);
            Graphics g = Graphics.FromImage(image);
            g.FillRectangle(new SolidBrush(invertColours ? Color.White : Color.Black), 0, 0, width, height);

            int curX = xPadding;
            int curY = yPadding;
			

            foreach(char character in text.ToUpper())
            {
                if (character == '\n')
                {
                    curY += 6;
                    curX = xPadding;
                    continue;
                }
                if (font.Characters.ContainsKey(character)) {
                    int[,] fontChar = font.Characters[character];
                    for(int i = 0; i < 5; i++)
                    {
						if (invertColours)
							image.SetPixel(curX, curY + i, Color.FromArgb(255 * (1 - fontChar[i, 0]), 255 * (1 - fontChar[i, 1]), 255 * (1 - fontChar[i, 2])));
						else
							image.SetPixel(curX, curY + i, Color.FromArgb(255 * fontChar[i, 0], 255 * fontChar[i, 1], 255 * fontChar[i, 2]));
					}
					curX += 2;
                }

            }
            return image;
        }
    }

{% endhighlight %}
