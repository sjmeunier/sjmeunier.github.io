---
layout: post
title:  "Barcodes in C#: UPC-5"
date:   2009-10-27 00:00:06
categories: Programming
tags: [c-sharp, tutorials, barcodes, programming]
---

UPC-5 is a variation on the UPC-A barcode](/programming/2009/10/barcodes-in-csharp-upca) designed to contain 5 digits of data.

![UPC-5](/assets/images/blog/barcodes/upc5.jpg){: .shadow-image .centered }

The checksum (or parity) for the UPC-5 format is used only to encode the barcode, and therefore is not coded directly.

Calculating the parity uses the same modulo 10 method as the UPC-A, except that the weightings are different. Odd-numbered positions are weighted 3, while even-numbered positions are weighted 9.

Once we have the weighted sum, we apply a modulo 10 to the weighted sum, which gets the remainder after applying a modulo of 10 to the weighted sum.

The parity is then 10 - (modulo 10 of the weighted sum).

UPC-5 only has a left guard and centre guard (which is places after the third digit), which are encoded as **1011** and **01** respectively.

The 5 digits are then encoded using either odd or even encoding, which is determined by the parity string.
<table border="0">
<tr>
<td>0</td>
<td><strong>00111</strong></td>
</tr>
<tr>
<td>1</td>
<td><strong>01011</strong></td>
</tr>
<tr>
<td>2</td>
<td><strong>01101</strong></td>
</tr>
<tr>
<td>3</td>
<td><strong>01110</strong></td>
</tr>
<tr>
<td>4</td>
<td><strong>10011</strong></td>
</tr>
<tr>
<td>5</td>
<td><strong>11001</strong></td>
</tr>
<tr>
<td>6</td>
<td><strong>11100</strong></td>
</tr>
<tr>
<td>7</td>
<td><strong>10101</strong></td>
</tr>
<tr>
<td>8</td>
<td><strong>10110</strong></td>
</tr>
<tr>
<td>9</td>
<td><strong>11010</strong></td>
</tr>
</table>

The odd and even encodings are the same as the ones for the [UPC-E format](/programming/2009/10/barcodes-in-csharp-upce)</p>
<table border="0">
<tr>
<td><strong>Digit</strong></td>
<td><strong>Odd</strong></td>
<td><strong>Even</strong></td>
</tr>
<tr>
<td>0</td>
<td><strong>0001101</strong></td>
<td><strong>0100111</strong></td>
</tr>
<tr>
<td>1</td>
<td><strong>0011001</strong></td>
<td><strong>0110011</strong></td>
</tr>
<tr>
<td>2</td>
<td><strong>0010011</strong></td>
<td><strong>0011011</strong></td>
</tr>
<tr>
<td>3</td>
<td><strong>0111101</strong></td>
<td><strong>0100001</strong></td>
</tr>
<tr>
<td>4</td>
<td><strong>0100011</strong></td>
<td><strong>0011101</strong></td>
</tr>
<tr>
<td>5</td>
<td><strong>0110001</strong></td>
<td><strong>0111001</strong></td>
</tr>
<tr>
<td>6</td>
<td><strong>0101111</strong></td>
<td><strong>0000101</strong></td>
</tr>
<tr>
<td>7</td>
<td><strong>0111011</strong></td>
<td><strong>0010001</strong></td>
</tr>
<tr>
<td>8</td>
<td><strong>0110111</strong></td>
<td><strong>0001001</strong></td>
</tr>
<tr>
<td>9</td>
<td><strong>0001011</strong></td>
<td><strong>0010111</strong></td>
</tr>
</table>
<p><span id="more-1182"></span><br />
<!--more-->

The full source code is available at [https://github.com/sjmeunier/barcoder](https://github.com/sjmeunier/barcoder).

{% highlight csharp %}
namespace BarcoderLib
{
    public class BarcodeUPC5 
    {
        private string gLeftGuard = "1011";
        private string gCentreGuard = "01";
        private string[] gOdd = { "0001101", "0011001", "0010011", "0111101", "0100011", "0110001", "0101111", "0111011", "0110111", "0001011" };
        private string[] gEven = { "0100111", "0110011", "0011011", "0100001", "0011101", "0111001", "0000101", "0010001", "0001001", "0010111" };
        private string[] gParity = {"00111", "01011", "01101", "01110", "10011", "11001", "11100", "10101", "10110", "11010"};

        private int[] gWeighting = { 3, 9, 3, 9, 3};
    
        public Bitmap Encode(string message)
        {
            string encodedMessage;

            Bitmap barcodeImage = new Bitmap(250, 100);
            Graphics g = Graphics.FromImage(barcodeImage);


            Validate(message);
            encodedMessage = EncodeBarcode(message);

            PrintBarcode(g, encodedMessage, message, 250, 100);

            return barcodeImage;
        }
        private void Validate(string message)
        {

            Regex reNum = new Regex(@"^\d+$");
            if (reNum.Match(message).Success == false)
            {
                throw new Exception("Encode string must be numeric");
            }

            if (message.Length != 5)
            {
                throw new Exception("Encode string must be 5 digits long");
            }
        }

        private void PrintBarcode(Graphics g, string encodedMessage, string message, int width, int height)
        {
            SolidBrush whiteBrush = new SolidBrush(Color.White);
            SolidBrush blackBrush = new SolidBrush(Color.Black);
            Font textFont = new Font(FontFamily.GenericMonospace, 10, FontStyle.Regular);
            g.FillRectangle(whiteBrush, 0, 0, width, height);

            int xPos = 20;
            int yTop = 20;
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
            yTop -= 17;
            for (int i = 0; i < message.Length; i++)
            {
                g.DrawString(message[i].ToString(), textFont, blackBrush, xPos, yTop);
                xPos += 7;
            }
        }

        private string EncodeBarcode(string message)
        {
            int i;
            int parityCode = CalcParity(message);
            char parity;
            string encodedString = gLeftGuard;

            for (i = 0; i < 5; i++)
            {
                parity = gParity[parityCode][i];
                if (parity == '1')
                {
                    encodedString += gOdd[Convert.ToInt32(message[i].ToString())];
                }
                else{
                    encodedString += gEven[Convert.ToInt32(message[i].ToString())];
                }
                if (i < 4)
                {
                    encodedString += gCentreGuard;
                }
            }

            return encodedString;
        }

        private int CalcParity(string message)
        {
            int sum = 0;
            int parity = 0;

            for (int i = 0; i < 5; i++)
            {
                sum += Convert.ToInt32(message[i].ToString()) * gWeighting[i];
            }

            parity = 10 - (sum % 10);
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
