---
layout: post
title:  "Barcodes in C#: Interleaved 2 of 5"
date:   2009-10-27 00:00:08
categories: Programming
tags: [c-sharp, tutorials, barcodes]
---

The interleaved 2 of 5 barcode uses an scheme which alternates using bars and spaces to encode data with narrow and wide bars (or spaces). The encoding is further split up into odd and even encodings.

![Interleaved 2 of 5](/assets/images/blog/barcodes/interleaved2of5.jpg){: .shadow-image .centered }

The interleaved 2 of 5 barcode does not require a checksum digit, and can be any length.

The left and right guard bars are **1010** and **01101** respectively, and the digts are encoded alternatively with the odd and even encodings.
<table border = "0">
<tr>
<td><strong>Digit</strong></td>
<td><strong>Odd</strong></td>
<td><strong>Even</strong></td>
</tr>
<tr>
<td>0</td>
<td><strong>1011001</strong></td>
<td><strong>0100110</strong></td>
</tr>
<tr>
<td>1</td>
<td><strong>1101011</strong></td>
<td><strong>0010100</strong></td>
</tr>
<tr>
<td>2</td>
<td><strong>1001011</strong></td>
<td><strong>0110100</strong></td>
</tr>
<tr>
<td>3</td>
<td><strong>1100101</strong></td>
<td><strong>0011010</strong></td>
</tr>
<tr>
<td>4</td>
<td><strong>1011011</strong></td>
<td><strong>0100100</strong></td>
</tr>
<tr>
<td>5</td>
<td><strong>1101101</strong></td>
<td><strong>0010010</strong></td>
</tr>
<tr>
<td>6</td>
<td><strong>1001101</strong></td>
<td><strong>0110010</strong></td>
</tr>
<tr>
<td>7</td>
<td><strong>1010011</strong></td>
<td><strong>0101100</strong></td>
</tr>
<tr>
<td>8</td>
<td><strong>1101001</strong></td>
<td><strong>0010110</strong></td>
</tr>
<tr>
<td>9</td>
<td><strong>1001001</strong></td>
<td><strong>0110110</strong></td>
</tr>
</table>
<!--more-->

The full source code is available at [https://github.com/sjmeunier/barcoder](https://github.com/sjmeunier/barcoder).

{% highlight csharp %}
namespace BarcoderLib
{
    public class BarcodeInter2of5
    {
        private string gLeftGuard = "1010";
        private string gRightGuard = "01101";
        private string[] gOdd = { "1011001", "1101011", "1001011", "1100101", "1011011", "1101101", "1001101", "1010011", "1101001", "1001001" };
        private string[] gEven = { "0100110", "0010100", "0110100", "0011010", "0100100", "0010010", "0110010", "0101100", "0010110", "0110110" };

        public Bitmap Encode(string message)
        {
            string encodedMessage;

            Bitmap barcodeImage = new Bitmap(250, 100);
            Graphics g = Graphics.FromImage(barcodeImage);


            Validate(message);
            encodedMessage = EncodeBarcode(message);

            PrintBarcode(g, encodedMessage, message, 350, 100);

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
            Font textFont = new Font(FontFamily.GenericMonospace, 10, FontStyle.Regular);
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
                if ((i % 2) == 0)
                {
                    encodedString += gOdd[Convert.ToInt32(message[i].ToString())];
                }
                else
                {
                    encodedString += gEven[Convert.ToInt32(message[i].ToString())];
                }
            }

            encodedString += gRightGuard;

            return encodedString;
        }

     }
}
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 27 Oct 2009_
