---
layout: post
title:  "Barcodes in C#: UPC-E"
date:   2009-10-27 00:00:04
categories: Programming
tags: [c-sharp, tutorials, barcodes]
---

The UPC-E barcode format is a compressed form of the [UPC-A format](/programming/2009/10/barcodes-in-csharp-upca) and is used where space is at a premium. The UPC-E format also require that there are at least 5 0's in a group in the barcode, so that it can be compressed.

![UPC-E](/assets/images/blog/barcodes/upce.jpg){: .shadow-image .centered }

Before we shorten the barcode, we need to enter a full 11 digit UPC-A barcode (excluding the parity digit), and then calculate the parity digit as per the UPC-A barcode.

We first need add up the 11 digits of the barcode with the weighting applied. The weighting is the same as the EAN-13 and UPC-A weighting, where the weighting of each digit which is in an odd-numbered position is 1 and for even-numbered positions, the weighting is 3. To find the sum, we need to multiply each digit by its weighting before adding it together.

Once we have the weighted sum, we apply a modulo 10 to the weighted sum, which gets the remainder after applying a modulo of 10 to the weighted sum.

The parity is then 10 - (modulo 10 of the weighted sum).

Now that we have an 12 digit number, we need to shorten it to an 8 digit number.

The rules for this are quite simple.

The manufacturer portion of the number is the first 5 digits excluding the first digit of the barcode. The product portion is the subsequent 5 digits.

If the last 3 manufacturer digits are 100, 200 or 000, then the shortened for is made up of the first digit, the first two digits of the manufacturer, the last 3 digits of the product, the third digit of the manufacturer and finally the parity.

Otherwise, if the last two digits of the manufacturer is 00, then the barcode is made up of first digit, the first three digits of the manufacturer, the last 2 digits of the product, the number 3, and finally the parity.

Then, if the last digit of the manufacturer is 0, the barcode is made up of first digit, the first four digits of the manufacturer, the last  digit of the product, the number 4, and finally the parity.

We can now use this number for the rest of the calculation

We do not split up this format, and only have with a guard bar at the beginning and end of the barcode. The left guard bars are encoded as **101**, while the right guard bar is **010101**.

The first digit of the barcode determines the parity set to use. The encodings are split up by odd and even encodings, which is determined by the parity bit. This is found by choosing the correct parity set encodings, and then finding the parity bit string for the parity we calculated earlier, and then returning the bit value at the position in the string for the digit we wish to encode.
<table border="0">
<tr>
<td><strong>Digit</stromg></td>
<td><strong>Set 0</strong></td>
<td><strong>Set 1</strong></td>
</tr>
<tr>
<td>0</td>
<td><strong>000111</strong></td>
<td><strong>111000</strong></td>
</tr>
<tr>
<td>1</td>
<td><strong>001011</strong></td>
<td><strong>110100</strong></td>
</tr>
<tr>
<td>2</td>
<td><strong>001101</strong></td>
<td><strong>110010</strong></td>
</tr>
<tr>
<td>3</td>
<td><strong>001110</strong></td>
<td><strong>110001</strong></td>
</tr>
<tr>
<td>4</td>
<td><strong>010011</strong></td>
<td><strong>101100</strong></td>
</tr>
<tr>
<td>5</td>
<td><strong>011001</strong></td>
<td><strong>100110</strong></td>
</tr>
<tr>
<td>6</td>
<td><strong>011100</strong></td>
<td><strong>100011</strong></td>
</tr>
<tr>
<td>7</td>
<td><strong>010101</strong></td>
<td><strong>101010</strong></td>
</tr>
<tr>
<td>8</td>
<td><strong>010110</strong></td>
<td><strong>101001</strong></td>
</tr>
<tr>
<td>9</td>
<td><strong>011010</strong></td>
<td><strong>100101</strong></td>
</tr>
</table>

Once we have the parity bit, we then choose the odd or even encoding accordingly, for each digit. Only 6 digits are explicitly encoded, since the first and last digits are used to calculate the parity bit, so are not needed to be encoded directly.
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
<!--more-->

The full source code is available at [https://github.com/sjmeunier/barcoder](https://github.com/sjmeunier/barcoder).

{% highlight csharp %}
namespace BarcoderLib
{
    public class BarcodeUPCE
    {
        private string gLeftGuard = "101";
        private string gRightGuard = "010101";
        private string[] gOdd = { "0001101", "0011001", "0010011", "0111101", "0100011", "0110001", "0101111", "0111011", "0110111", "0001011" };
        private string[] gEven = { "0100111", "0110011", "0011011", "0100001", "0011101", "0111001", "0000101", "0010001", "0001001", "0010111" };
        private int[] gWeighting = { 3, 1, 3, 1, 3, 1, 3, 1, 3, 1, 3 };
        private string gLongBars = "111000000000000000000000000000000000000000000111111";
        private string[] gParity0 = { "000111", "001011", "001101", "001110", "010011", "011001", "011100", "010101", "010110", "011010"};
        private string[] gParity1 = { "111000", "110100", "110010", "110001", "101100", "100110", "100011", "101010", "101001", "100101"};

        public Bitmap Encode(string message)
        {
            string encodedMessage;
            string fullMessage;
            string shortMessage;

            Bitmap barcodeImage = new Bitmap(250, 100);
            Graphics g = Graphics.FromImage(barcodeImage);


            Validate(message);
            fullMessage = message + CalcParity(message).ToString().Trim();
            shortMessage = ConvertToShort(fullMessage);
            encodedMessage = EncodeBarcode(shortMessage, fullMessage);

            PrintBarcode(g, encodedMessage, shortMessage, fullMessage, 250, 100);

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

        private void PrintBarcode(Graphics g, string encodedMessage, string message, string fullMessage, int width, int height)
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
            g.DrawString(fullMessage[0].ToString().Trim(), textFont, blackBrush, xPos - 10, yTop);

            xPos += 1;
            for (int i = 0; i < 6; i++)
            {
                g.DrawString(message[i].ToString().Trim(), textFont, blackBrush, xPos, yTop);
                xPos += 7;
            }
            xPos += 7;
            g.DrawString(message[6].ToString().Trim(), textFont, blackBrush, xPos, yTop);

        }

        private string EncodeBarcode(string message, string fullMessage)
        {
            int i;
            string encodedString = gLeftGuard;
            int parityCode = Convert.ToInt32(fullMessage[11].ToString());
            int paritySet = Convert.ToInt32(fullMessage[0].ToString());
            char parity;

            for (i = 0; i < 6; i++)
            {
                if (paritySet == 0)
                {
                    parity = gParity0[parityCode][i];
                }
                else
                {
                    parity = gParity1[parityCode][i];
                }
                if (parity == '1')
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
    
        private int CalcParity(string message)
        {
            int sum = 0;
            int parity = 0;

            for (int i = 0; i < message.Length; i++)
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

        private string ConvertToShort(string message)
        {
            string tmp = "";
            string manufacturer = message.Substring(1, 5);
            string product = message.Substring(6, 5);
            string digits = "";
            if ((manufacturer.Substring(2) == "000") || (manufacturer.Substring(2) == "100") || (manufacturer.Substring(2) == "200"))
            {
                digits = manufacturer.Substring(0, 2) + product.Substring(2, 3) + manufacturer.Substring(2, 1);
            }
            else if (manufacturer.Substring(3) == "00")
            {
                digits = manufacturer.Substring(0, 3) + product.Substring(3) + "3";
            }
            else if (manufacturer.Substring(4) == "0")
            {
                digits = manufacturer.Substring(0, 4) + product.Substring(4) + "4";

            }
            else
            {
                digits = manufacturer.Substring(0, 5) + product.Substring(4);
            }
            tmp = digits + message.Substring(11, 1);
            return tmp;
        }
            
    }
}
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 27 Oct 2009_
