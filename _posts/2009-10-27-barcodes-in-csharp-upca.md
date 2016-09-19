---
layout: post
title:  "Barcodes in C#: UPC-A"
date:   2009-10-27 00:00:03
categories: Programming
tags: [c-sharp, tutorials, barcodes]
---

The UPC-A barcode format is very similar to the [EAN-13 barcode](/programming/2009/10/barcodes-in-csharp-ean-13) and is mostly used in America, although in recent years, the US is moving towards the EAN-13 format too. The UPC-A format is 12 digits long, with the last digit being the checksum (or parity) digit.

![UPC-A](/assets/images/blog/barcodes/upca.jpg){: .shadow-image .centered }

To calculate the parity for UPC-A, we use the same process as with the EAN-13 barcode. We first need add up the 11 digits of the barcode with the weighting applied. The weighting is the same as the EAN-13 weighting, where the weighting of each digit which is in an odd-numbered position is 1 and for even-numbered positions, the weighting is 3. To find the sum, we need to multiply each digit by its weighting before adding it together.

Once we have the weighted sum, we apply a modulo 10 to the weighted sum, which gets the remainder after applying a modulo of 10 to the weighted sum.

The parity is then 10 - (modulo 10 of the weighted sum).

Now that we have an 12 digit number, we can encode it. 

We split the UPC-A barcode into 2 blocks of 6 digits each, with a guard bar at the beginning and end of the barcode and one in the middle of the barcode too. The end guard bars are encoded as **101**, while the middle guard bar is **01010**.

In the EAN-13 barcode format, the parity digit was not encoded directly, with only the first 12 digits being encoded, whereas with UPC-A, all 12 digits are encoded, including the parity. This simplifies the encoding process, since we do not have to worry about odd and even parity encodings.

The left hand side encodings are identical to the left hand odd parity encodings from EAN-13.
<table border="0">
<tr>
<td>0</td>
<td><strong>0001101</strong></td>
</tr>
<tr>
<td>1</td>
<td><strong>0011001</strong></td>
</tr>
<tr>
<td>2</td>
<td><strong>0010011</strong></td>
</tr>
<tr>
<td>3</td>
<td><strong>0111101</strong></td>
</tr>
<tr>
<td>4</td>
<td><strong>0100011</strong></td>
</tr>
<tr>
<td>5</td>
<td><strong>0110001</strong></td>
</tr>
<tr>
<td>6</td>
<td><strong>0101111</strong></td>
</tr>
<tr>
<td>7</td>
<td><strong>0111011</strong></td>
</tr>
<tr>
<td>8</td>
<td><strong>0110111</strong></td>
</tr>
<tr>
<td>9</td>
<td><strong>0001011</strong></td>
</tr>
</table>

These encodings ensure that UPC-A forms a subset of the EAN-13 barcode.

The right hand side encodings (the last 6 digits) are the same as the right hand codings of EAN-13. The encodings are as follows:
<table border="0">
<tr>
<td>0</td>
<td><strong>1110010</strong></td>
</tr>
<tr>
<td>1</td>
<td><strong>1100110</strong></td>
</tr>
<tr>
<td>2</td>
<td><strong>1101100</strong></td>
</tr>
<tr>
<td>3</td>
<td><strong>1000010</strong></td>
</tr>
<tr>
<td>4</td>
<td><strong>1011100</strong></td>
</tr>
<tr>
<td>5</td>
<td><strong>1001110</strong></td>
</tr>
<tr>
<td>6</td>
<td><strong>1010000</strong></td>
</tr>
<tr>
<td>7</td>
<td><strong>1000100</strong></td>
</tr>
<tr>
<td>8</td>
<td><strong>1001000</strong></td>
</tr>
<tr>
<td>9</td>
<td><strong>1110100</strong></td>
</tr>
</table>
<!--more-->

The full source code is available at [https://github.com/sjmeunier/barcoder](https://github.com/sjmeunier/barcoder).

{% highlight csharp %}
namespace BarcoderLib
{
    public class BarcodeUPCA
    {
        private string gLeftGuard = "101";
        private string gCentreGuard = "01010";
        private string gRightGuard = "101";
        private string[] gLH = { "0001101", "0011001", "0010011", "0111101", "0100011", "0110001", "0101111", "0111011", "0110111", "0001011" };
        private string[] gRH = { "1110010", "1100110", "1101100", "1000010", "1011100", "1001110", "1010000", "1000100", "1001000", "1110100" };
        private int[] gWeighting = { 3, 1, 3, 1, 3, 1, 3, 1, 3, 1, 3 };
        private string gLongBars = "11111111110000000000000000000000000000000000011111000000000000000000000000000000000001111111111";

  

        public Bitmap Encode(string message)
        {
            string encodedMessage;
            string fullMessage;

            Bitmap barcodeImage = new Bitmap(250, 100);
            Graphics g = Graphics.FromImage(barcodeImage);


            Validate(message);
            fullMessage = message + CalcParity(message).ToString().Trim();
            encodedMessage = EncodeBarcode(fullMessage);

            PrintBarcode(g, encodedMessage, fullMessage, 250, 100);

            return barcodeImage;
        }
        private void Validate(string message)
        {

            Regex reNum = new Regex(@"^\d+$");
            if (reNum.Match(message).Success == false)
            {
                throw new Exception("Encode string must be numeric");
            }

            if (message.Length != 11)
            {
                throw new Exception("Encode string must be 11 digits long");
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
            int barGuardHeight = 7;

            for (int i = 0; i < encodedMessage.Length; i++)
            {
                if (encodedMessage[i] == '1')
                {
                    if (gLongBars[i] == '1')
                    {
                        g.FillRectangle(blackBrush, xPos, yTop, 1, barHeight + barGuardHeight);
                    }
                    else
                    {
                        g.FillRectangle(blackBrush, xPos, yTop, 1, barHeight);
                    }
                }
                xPos += 1;
            }
            
            xPos = 20;
            yTop += barHeight - 2;
            g.DrawString(message[0].ToString().Trim(), textFont, blackBrush, xPos - 10, yTop);

            xPos += 8;
            for (int i = 1; i < 6; i++)
            {
                g.DrawString(message[i].ToString().Trim(), textFont, blackBrush, xPos, yTop);
                xPos += 7;
            }
            xPos += 4;

            for (int i = 6; i <11; i++)
            {
                g.DrawString(message[i].ToString().Trim(), textFont, blackBrush, xPos, yTop);
                xPos += 7;
            }
            xPos += 11;
            g.DrawString(message[11].ToString().Trim(), textFont, blackBrush, xPos, yTop);

        }

        private string EncodeBarcode(string message)
        {
            int i;
            string encodedString = gLeftGuard;

            for (i = 0; i < 6; i++)
            {
                encodedString += gLH[Convert.ToInt32(message[i].ToString())];
            }
            encodedString += gCentreGuard;

            for (i = 6; i < 12; i++)
            {
                encodedString += gRH[Convert.ToInt32(message[i].ToString())];
            }
            encodedString += gRightGuard;

            return encodedString;
        }

        private int CalcParity(string message)
        {
            int sum = 0;
            int parity = 0;

            for (int i = 0; i < 11; i++)
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
