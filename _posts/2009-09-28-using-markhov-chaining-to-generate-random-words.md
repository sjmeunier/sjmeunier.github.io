---
layout: post
title:  "Using Markhov Chaining to Generate Random Words"
date:   2009-09-28 00:00:00
categories: Programming
tags: [c-sharp, tutorials, programming]
---

A while back I wrote a little application that performs Markhov chaining to create random names.

What Markhov chaining does is creates new words based on the probability of a particular letter or syllable following another one, which is calculated based on a list of words used for this purpose.

So for example, if you wanted to create random names which sounded Italian, or Greek, then you would provide a list of real Italian or Greek words, and then the application would generate random new words which had a similar feel. The output words retain the flavour of the input words. This works for different languages too. If you enter a lot of German words, then the words generated will sound very German too (even if they are not real words).

Now this can work either on a letter or syllable basis, but the principle is the same for both, where for syllables, you would split the word into syllables rather than letters, but I will work on letters for the rest of this post.

So, let's look at an example of how this would work.

We have this list of words:
house
fallen
horse
raisin

To process the input words, we loop over the words, and for each word, we loop through each letter in the word. So for each letter in the word now, we add current letter to an array, and keep track of the number of occurrences we have had of that letter, and then look at the following letter, and then for main letter we add it to our array of letters following that letter with the number of occurrences.

To make that clearer, using our sample words, after processing the word house our array will look as follows (with number of occurrences in brackets).
h(1) -> o(1)
o(1) -> u(1)
u(1) -> s(1)
e(1) -> ~(1)

Here we used ~ to denote that that letter had no following letters.

Now processing the rest of the words we end up with
h(2) -> o(2)
o(2) -> u(1)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;r(1)

u(1) -> s(1)
s(3) -> e(2)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;i(1)
e(3) -> ~(2)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;n(1)
f(1)  -> a(1)
a(2)  -> l(1)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;i(1)
l(1)   -> l(1)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;e(1)
n(2)  -> ~(2)
r(2)   -> s(1)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a(1)
i(2)    -> s(1)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;n(1)

Now, when you generate a word, you can randomly choose an initial letter, and then using a random number generator, select which child letter to use as the next letter, and then randomly choose a child letter for that letter, and so on, until you reach an end of word character.

The source for the Markhov chaining application which I have written in C# is available on [GitHub](https://github.com/sjmeunier/markhov-word-generator).

_Originally posted on my old blog, Smoky Cogs, on 28 Sep 2009_
