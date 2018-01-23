---
layout: post
title:  "A Recent Items List in Java"
date:   2018-01-23 18:00:00
categories: Programming
tags: [java, android, programming]
---

Over the last few months, I have been tinkering around with a genealogy app for Java - which you can find at [https://github.com/sjmeunier/arbor-familiae](https://github.com/sjmeunier/arbor-familiae).

One of the features I implemented was a list of recently viewed individuals, and while implementing that I discovered a few useful methods in Java's list classes, which made this a piece of cake.

The principle of a recent list is a simple list, constrained by a maximum size, where new items are added to the front of the list (index 0), and any items exceeding the max length are removed.

One tricky thing that popped up, is that when adding an item to the list, it should check that the item is not already in the list. If so, the item should be moved to the front of the list, and only one instance of that value should ever be in the list.

In the code below, almost all the magic happens in the _addItem()_ method. This method, as described above, removes an existing item, if it already occurs, adds the item at position 0, and finally removes the last item if the size exceeds the maximum. 

This can all be implemented using arrays, instead of a list, but the extra methods of being able to add or remove items at certain positions without having to update the rest of the list independantly makes lists a very clean implementation.
<!--more-->


{% highlight java %}
import java.util.ArrayList;

public class RecentList {
    private ArrayList items;
    private int maxSize;

    public RecentList(int maxSize) {
        items = new ArrayList();
        this.maxSize = maxSize;
    }

    public RecentList(int maxSize, int[] initialItems) {
        items = new ArrayList();
        this.maxSize = maxSize;
        for(int i = 0; i < initialItems.length; i++) {
            items.add(initialItems[i]);
        }
    }

    public void addItem(int item) {
        if (item < 1)
            return;
        //check if already in list and if so remove it
        int position = items.indexOf(item);
        if (position >= 0)
            items.remove(position);
        items.add(0, item);

        if (items.size() > maxSize)
            items.remove(items.size() - 1);
    }

    public int getSize() {
        return items.size();
    }

    public void clear() {
        items.clear();
    }

    public int[] getItems() {
        int[] returnItems = new int[items.size()];
        for(int i = 0; i < items.size(); i++)
            returnItems[i] = (int)items.get(i);
        return returnItems;
    }
}
{% endhighlight %}

