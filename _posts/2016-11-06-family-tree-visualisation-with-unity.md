---
layout: post
title:  "Family Tree Visualisation with Unity 3D"
date:   2016-11-06 11:00:03
categories: Programming
tags: [c-sharp, unity, genealogy, programming]
---

Genealogy has a rather interesting problem. Due to the fact that every person has 2 parents, each generation you go back in time, the number of individuals at that generation increases by a factor of 2 each time. Even at 10 generations back you have 1024 individuals. This poses a bit of a problem when trying to view the data. Most genealogical software only shows you several generations in one view, which, is all that can fit on a screen, which leads to only being able to see small parts of your family tree at a time.

This is where AncestryWeb3D comes in. I chose to write this application in Unity 3D for two reasons. Firstly, I hadwanted to learn how to program with Unity, and secondly, it would give a great way to visually draw the tree.

In the tree, individuals are represented by spheres - red for female and blue for male ancestors. Ancestors with no known parents have a green semi-transparent shell to indicate dead-ends. The individuals are connected by lines of descent.

The application loads a standard GEDCOM file making it useful to visualise any tree.
<!--more-->

Each generation is normalised to fit on a circle, and evenly spaced. This is to help with space. While the number of potential individuals at a certain generations increases exponentially, in reality, there are not nearly so many known individuals at a certain generation. This is due to intermarriage amongst ancestors, as well as dead ends. This means there is a managable number of individuals at each generation, and can therefore be shown fairly easily. The size of the spheres also indicates the number of times that individual appears in the family tree, which indicates the number of lines of decent between the root person and that individual. The biger the sphere, the more lines of decent.

This type of visualisation is very good at showing where the tree intertwines. No person's family history is straightforward. Our ancestors all intermingled in very complex ways, with many intermariages and common lines of descent.

Hovering the mouse cursor over a sphere gives more information on the individual. The camera can be moved around by the arrow keys, and rotated by holding down the left mouse button, and panned by holding down the right mouse button.

I have open-sourced the code, so enjoy playing around with it.

The full source code is available at [https://github.com/sjmeunier/ancestry-web-3d](https://github.com/sjmeunier/ancestry-web-3d).

![AncestryWeb3D](/assets/images/blog/ancestryweb.png){: .shadow-image .centered }

