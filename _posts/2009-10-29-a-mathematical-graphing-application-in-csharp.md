---
layout: post
title:  "A Mathematical Graphing Application in C#"
date:   2009-10-29 00:00:00
categories: Programming
tags: [c-sharp, tutorials, maths, programming]
---

![Grapher](/assets/images/blog/grapher.jpg){: .shadow-image .right }Creating an application to draw graphs of mathematical equations has two components to it, namely, parsing the equation and drawing the results.

Parsing an equation is quite a complicated endeavor. However, I found on the internet a very useful C&$35; library that parses equations very well called dotMath by Steve Hebert. You can visit the website for the library at [http://www.codeplex.com/dotMath](http://www.codeplex.com/dotMath). Rather than write my own library, I used this library to do all the parsing for me.

The fun part - for me anyway - is trying to draw the graph of an expression. I created a Windows Forms control that contains a picture box that fills the entire control. This picture box will contain the graph that we draw.

The actual generation of the image we are going to draw happens using a **Bitmap** object. 

We set up the pens and brushes we need for the axis, graph, grid and background using the configuration settings that the class contains.

If the flag to draw the axes is set, we first draw the x and y axes, and then label it, using the scale we specify. The offset allows us to move the graph so that the centre of the screen does not need to be the position (0,0), which is the default. The LabelIncrement determines how many pixels apart the labels need to be. If we set the draw grid flag to true, we also draw a grid, which will make it easier to see what value the graph shows. Drawing the axes and grid works by taking the centre point and working out along each axis to the end of the screen.
<!--more-->

Now that we have set everything up, we can draw the graph. Here is where we use the **dotMath** class to parse our equation we want to draw.

So given an example equation of something like **x*x + 3 * X + 23** or **sin(x)**, we iterate through each pixel, and get the value of x at that pixel based on the scale and offset.

We then replace the character **x** in the equation with our value for x, and pass the string to the equation parser, which then calculates the y value.

From this we can, again using the scale and offset, determine the y value of the coordinate we need. To actually draw the point, we draw a line from the previous pixel to the current pixel which we have calculated.

Here is the code for the graph drawing class. 

{% highlight csharp %}
using System;
using System.ComponentModel;
using System.Drawing;
using System.Windows.Forms;

namespace EquationGrapher
{
    public partial class Graph : UserControl
    {
        public Color axisColor = Color.Black;
        public Color drawColor = Color.Red;
        public Color gridColor = Color.LightGray;
        public Color backgroundColor = Color.White;
        public int axisFontSize = 5;
        public Boolean drawAxes = true;
        public Boolean drawGrid = true;
        
        public Graph()
        {
            InitializeComponent();
        }

        public Bitmap GenerateGraph(string equation, double xScale, double yScale, double xOffset, double yOffset, double xLabelIncrement, double yLabelIncrement, int width, int height)
        {
            Bitmap image = new Bitmap(width, height);
            int midX = width / 2;
            int midY = height / 2;

            Graphics g = Graphics.FromImage(image);
            SolidBrush backgroundBrush = new SolidBrush(backgroundColor);
            SolidBrush axisBrush = new SolidBrush(axisColor);
            SolidBrush drawBrush = new SolidBrush(drawColor);
            SolidBrush gridBrush = new SolidBrush(gridColor);

            Pen drawPen = new Pen(drawColor);
            Pen axisPen = new Pen(axisColor);
            Pen gridPen = new Pen(gridColor);
            Font axisFont = new Font(FontFamily.GenericSansSerif, axisFontSize);
            //Clear background
            g.FillRectangle(backgroundBrush, 0, 0, width, height);
            
            //Draw Axes
            if (drawAxes)
            {
                g.DrawLine(axisPen, 0, midY, width, midY);
                g.DrawLine(axisPen, midX, 0, midX, height);

                //draw labels
                float xIncrementSize = (float)(xScale * xLabelIncrement);

                float curX = midX;
                while (curX < width)
                {
                    if (curX != midX)
                    {
                        if (drawGrid)
                        {
                            g.DrawLine(gridPen, curX, 0, curX, height);
                        }
                        g.DrawLine(axisPen, curX, midY, curX, midY + 3);
                        g.DrawString(Math.Round((curX - midX) * xScale + xOffset, 2).ToString(), axisFont, axisBrush, curX - 3, midY + 5);
                    }
                    curX += xIncrementSize;
                }
                curX = midX;
                while (curX > 0)
                {
                    if (curX != midX)
                    {
                        if (drawGrid)
                        {
                            g.DrawLine(gridPen, curX, 0, curX, height);
                        }
                        g.DrawLine(axisPen, curX, midY, curX, midY + 3);
                        g.DrawString(Math.Round((curX - midX) * xScale + xOffset, 2).ToString(), axisFont, axisBrush, curX - 3, midY + 5);
                    }
                    curX -= xIncrementSize;

                }

                float yIncrementSize = (float)(yScale * yLabelIncrement);

                float curY = midY;
                while (curY < height)
                {
                    if (curY != midY)
                    {
                        if (drawGrid)
                        {
                            g.DrawLine(gridPen, 0, curY, width, curY);
                        }
                        g.DrawLine(axisPen, midX, curY, midX + 3, curY);
                        g.DrawString(Math.Round(((curY - midY) * yScale + yOffset) * -1, 2).ToString(), axisFont, axisBrush, midX + 5, curY - 3);
                    }
                    curY += yIncrementSize;
                }
                curY = midY;
                while (curY > 0)
                {
                    if (curY != midY)
                    {
                        if (drawGrid)
                        {
                            g.DrawLine(gridPen, 0, curY, width, curY);
                        }
                        g.DrawLine(axisPen, midX, curY, midX + 3, curY);
                        g.DrawString(Math.Round(((curY - midY) * yScale + yOffset) * -1, 2).ToString(), axisFont, axisBrush, midX + 5, curY - 3);
                    }
                    curY -= yIncrementSize;
                }

            }

            //Draw graph
            float prevX = 0;
            float prevY = 0;
            double x = 0;
            double y = 0;
            float xImage = 0;
            float yImage = 0;
            string filledEquation = "";

            for (int i = 0; i < width; i++)
            {
                x = ((double)(i - midX) * xScale) + xOffset;
                
                filledEquation = equation.Replace("x", x.ToString());
                dotMath.EqCompiler equationCompiler = new dotMath.EqCompiler(filledEquation, true);
                equationCompiler.Compile();
                y = equationCompiler.Calculate() + yOffset;

                xImage = i;
                yImage = (int)(y * (-1) / yScale) + midY;
                if (i > 0)
                {
                    g.DrawLine(drawPen, prevX, prevY, xImage, yImage);
                }
                prevX = xImage;
                prevY = yImage;
            }
            return image;
        }
        public void DrawGraph(string equation, double xScale, double yScale, double xOffset, double yOffset, double xLabelIncrement, double yLabelIncrement)
        {
            picGraph.Image = GenerateGraph(equation, xScale, yScale, xOffset, yOffset, xLabelIncrement, yLabelIncrement, picGraph.Width, picGraph.Height);
        }
    }
}
{% endhighlight %}

The full source code is available at [https://github.com/sjmeunier/grapher](https://github.com/sjmeunier/grapher).

_Originally posted on my old blog, Smoky Cogs, on 29 Oct 2009_
