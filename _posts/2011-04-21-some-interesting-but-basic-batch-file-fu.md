---
layout: post
title:  "Some Interesting (But Basic) Batch-file Fu"
date:   2011-04-21 00:00:00
categories: Programming
tags: [windows, tutorials]
---

I haven't made much use of Windows batch files for many, many years. I just haven't had a need for it, but I learnt a few interesting titbits of Batch file use that I never really knew before today.

**Putting IFs into FOR loops**
Firstly, batch files support FOR loops as well as IF statements, and therefore putting this together, I would expect this piece of code to work:
<!--more-->

{% highlight javascript %}
for /D %%d in (*) do (
  if "%%d" == "a" goto A 
  if "%%d" == "b" goto B

  echo "Default - %%d"
  goto CONTINUE

  :A
  echo "A - %%d"
  goto CONTINUE

  :B
  echo "B - %%d"
  goto CONTINUE

  :CONTINUE
)
{% endhighlight %}

At first glance this code should work. It is merely a few IF statements inside the for loop, which loop through all the directories inside the current directory, and then prints a message following the logic of the if statements.

This code fails (at least while running on Windows 7) because, while it will work for the first item, after the goto statements, the for loop decides to stop working.

To get around this, the solution I used was to move the logic inside the for loop into a separate batch file, and then using the CALL command to execute the second batch file from within the FOR loop. This works a charm.

_First batch file_
{% highlight text %}
for /D %%d in (*) do (
 	CALL SecondFile.bat %%d
)
{% endhighlight %}

_Second batch file_
{% highlight text %}
if "%1" == "a" goto A 
if "%1" == "b" goto B

echo "Default - %1"
goto CONTINUE

:A
echo "A - %1"
goto CONTINUE

:B
echo "B - %1"
goto CONTINUE

:CONTINUE
)
{% endhighlight %}

**Substring in Batch files**
What I also wanted to do, was only compare a portion of the directory name, so then discovered that Batch files have a very simple method of substringing.

The basic format is _%varname:~[x],[y]%_, where _[x]_ is the starting index in the string, and _[y]_ determines the number of characters to select, which is optional.

If the first parameter is positive, then it denotes the index from the start of the string, while if it is negative, it will denote the index from the end of the string.

So let us use an example

{% highlight text %}
set var=teststring

echo %var:~0,5%
echo %var:~-3%
echo %var:~-5,2%
echo %var:~4%
{% endhighlight %}

This will output the following output

{% highlight text %}
tests
ing
tr
string
{% endhighlight %}

_Originally posted on my old blog, Smoky Cogs, on 21 Apr 2011_
