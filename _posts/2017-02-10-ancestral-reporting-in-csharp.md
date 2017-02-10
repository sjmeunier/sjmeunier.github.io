---
layout: post
title:  "Ancestral Reporting in C#"
date:   2017-02-10 19:00:03
categories: Programming
tags: [c-sharp, genealogy, programming]
---

A while back, I [posted](https://sjmeunier.github.io/programming/2016/11/06/family-tree-visualisation-with-unity3d.html) about the family tree visualisation app I wrote using Unity. The 3D world is ideal to visualise how the individuals relate to each other, but it does not replace good old-fashioned ancestral and descendant reports.

As an expansion on the Unity project, I have created a new Windows project in C# based on the code from the Unity project, which allows you to generate one of several reports based on an imported GEDCOM file.

The GEDCOM parser and C# data structure is essentially identical to the Unity project. Once the data has been imported, we process the data according to the report we want.

The following reports are implemented in the application:

*Full Ancestry - Generates a detailed ancestry of the root individual, iterating through each generation back in the tree as far as it goes or until it reaches the maximum depth set. The data includes date and place of birth and death, spouses and children of the individual.

*Abridged Ancestry - Generates a more abridged ancestry of the root individual. In this report, all ancestors are shown, as for the full ancestry report, but it shows a lot less information, making the report more compact.

*Leaf Ancestors - This report only includes ancestors which have an unknown parent in the tree. This is useful to find out quickly which ancestors need more research to find new ancestors. This gives the full information for the individual, much like the Full Ancestry report

*Abridged Leaf Ancestors - The same as the above report, but just like the Abridged Ancestry report, it gives less information on individuals.

*Generation Summary - This report lists the number of unique individuals per ancestral generation.

*Descendant Report - This is the opposite of the Full Ancestry report. It lists all descendants of the root individual.

*Ancestor Place Report - Generates a list of places listed in the GEDCOM, listing all the individuals with a birth or death associated with that place

*Shared Y-DNA Report - This report finds the most distant paternal ancestor of the root individual, and then generates a descendant report of that ancestor, listing all male descendants, which all share the same Y-DNA.

*Shared mt-DNA Report - Similar to the Y-DNA report, this report finds the most distant maternal ancestor, and then generates a descendant report listing all descendants which share the same mt-DNA. This report includes both male and female children, but only recurses deeper into the female branches of the tree, since mt-DNA is only passed along the female line.

<!--more-->
The source for the project, like the Unity project, is open-source, so feel free to use the code any way you like.

The full source code for Ancestry-Reporter is available at [https://github.com/sjmeunier/ancestry-reporter](https://github.com/sjmeunier/ancestry-reporter).
The full source code for the Unity app is available at [https://github.com/sjmeunier/ancestry-web-3d](https://github.com/sjmeunier/ancestry-web-3d).