---
layout: post
title:  "Barcodes in C#: Standard 2 of 5"
date:   2009-10-27 00:00:07
categories: Programming
tags: [c-sharp, tutorials, barcodes]
---

The standard 2 of 5 barcode format encodes all of the information in the bars, and only uses the spaces for spacing purposes, unlike all the other barcodes we have looked at so far, and is used mostly in warehouses and airline ticketing.

![Standard 2 of 5](/assets/images/blog/barcodes/standard2of5.jpg){: .shadow-image .centered }

To calculate the checksum digit, working through the digits from right to left, you add up the odd-positioned elements and even-positioned elements separately. We then find the checksum by 10 - ((((3 * odd) + even) modulo 10) modulo 10).

Using this full string then we apply the encoding, where a narrow bar is defined by **11** and a wide bar with **111111**. The left guard is **1111011110110** and the right guard is **1111011011110**.

The encodings for each digit is as follows
<table border="0">
<tr>
<td>0</td>
<td><strong>11011011111101111110110</strong></td>
</tr>
<tr>
<td>1</td>
<td><strong>11111101101101101111110</strong></td>
</tr>
<tr>
<td>2</td>
<td><strong>11011111101101101111110</strong></td>
</tr>
<tr>
<td>3</td>
<td><strong>11111101111110110110110</strong></td>
</tr>
<tr>
<td>4</td>
<td><strong>11011011111101101111110</strong></td>
</tr>
<tr>
<td>5</td>
<td><strong>11111101101111110110110</strong></td>
</tr>
<tr>
<td>6</td>
<td><strong>11011111101111110110110</strong></td>
</tr>
<tr>
<td>7</td>
<td><strong>11011011011111101111110</strong></td>
</tr>
<tr>
<td>8</td>
<td><strong>11111101101101111110110</strong></td>
</tr>
<tr>
<td>9</td>
<td><strong>11011111101101111110110</strong></td>
</tr>
</table>
<!--more-->

The full source code is available at [https://github.com/sjmeunier/barcoder](https://github.com/sjmeunier/barcoder).

{% highlight csharp %}
namespace BarcoderLib
{
    public class BarcodeStandard2of5
    {
        private string gLeftGuard = "1111011110110";
        private string gRightGuard = "1111011011110";
        private string[] gCoding = { "11011011111101111110110", "11111101101101101111110", "11011111101101101111110", 
                                     "11111101111110110110110", "11011011111101101111110", "11111101101111110110110",
                                     "11011111101111110110110", "11011011011111101111110", "11111101101101111110110", "11011111101101111110110" };
        public Bitmap Encode(string message)
        {
            string encodedMessage;
            string fullMessage;

            Bitmap barcodeImage = new Bitmap(400, 100);
            Graphics g = Graphics.FromImage(barcodeImage);

            Validate(message);

            fullMessage = message + CalcParity(message).ToString().Trim();
            encodedMessage = EncodeBarcode(fullMessage);

            PrintBarcode(g, encodedMessage, fullMessage, 350, 100);

            return barcodeImage;
        }
        private void Validate(string message)
        {

            Regex reNum = new Regex(@"^\d+$");
            if (reNum.Match(message).Success == false)
            {
                throw new Exception("Encode string must be numeric");
            }

        }

        private void PrintBarcode(Graphics g, string encodedMessage, string message, int width, int height)
        {
            SolidBrush whiteBrush = new SolidBrush(Color.White);
            SolidBrush blackBrush = new SolidBrush(Color.Black);
            Font textFont = new Font(FontFamily.GenericMonospace, 12, FontStyle.Regular);
            g.FillRectangle(whiteBrush, 0, 0, width, height);

            int xPos = 20;
            int yTop = 10;
            int barHeight = 50;

            for (int i = 0; i < encodedMessage.Length; i++)
            {
                if (encodedMessage[i] == '1')
                {
                    g.FillRectangle(blackBrush, xPos, yTop, 1, barHeight);
                }
                xPos += 1;
            }

            xPos = 20;
            yTop += barHeight - 2;
            for (int i = 0; i < message.Length; i++)
            {
                g.DrawString(message[i].ToString().Trim(), textFont, blackBrush, xPos, yTop);
                xPos += 7;
            }
        }

        private string EncodeBarcode(string message)
        {
            int i;
            string encodedString = gLeftGuard;

            for (i = 0; i < message.Length; i++)
            {
                encodedString += gCoding[Convert.ToInt32(message[i].ToString())];
            }

            encodedString += gRightGuard;

            return encodedString;
        }

        private int CalcParity(string message)
        {
            int oddSum = 0;
            int evenSum = 0;
            int parity = 0;

            for (int i = message.Length - 1; i >= 0; i--)
            {
                if ((i % 2) == 0)
                {
                    evenSum += Convert.ToInt32(message[i].ToString());
                }else
                {
                    oddSum += Convert.ToInt32(message[i].ToString());
                }
            }

            parity = 10 - ((((3 * oddSum) + evenSum) % 10) % 10);
            if (parity == 10)
            {
                parity = 0;
            }
            return parity;
        }
    }
}
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 27 Oct 2009_
