---
layout: post
title:  "Unveiling Arbor Familiae"
date:   2018-02-02 18:00:00
categories: Programming
tags: [java, android, programming]
---

As I mentioned in my previous [post](https://sjmeunier.github.io/programming/2018/01/23/a-recent-items-list-in-java.html), I have been creating a family tree viewer for Android as a hooby project, and have reached a point now, where it is in a useful state. Therefore, Arbor Familiae is now available on the [Google Play store](https://play.google.com/store/apps/details?id=com.sjmeunier.arborfamiliae).

I have open-sourced the project, and released the app as a completely free app on the store. The source code can be found on my Github account at [https://github.com/sjmeunier/arbor-familiae](https://github.com/sjmeunier/arbor-familiae).

The app allows you to import GEDCOM files into the application, and multiple GEDCOMs can be imported into separate trees.

With an imported tree, the follwing features are available
* _Tree Chart_ - a chart showing all ancestors up to the configured number of generations of the active individual in a tree form, and also includes spouses and children of the active individual.
* _Fan Chart_ - shows the ancestors or descendants of the active individual up to the configured number of generations, as a fan chart
* _Relationship Chart_ - shows the relationship between the root individual and the active individual.
* _Heatmap_ - shows the locations for birth, death, burial and baptism events for the ancestors of the active individual mapped as a heatmap using Google Maps.
* _Detailed Biography_ - shows detailed information on the active individual, including birth, death, parents, souses and children, as well as any notes or sources included in the original GEDCOM for the individual.

![Arbor Familiae](/assets/images/blog/arbor-familiae-screenshot.png){: .shadow-image .centered }

I will be posting more technical blog posts in the future, highlighting some of the interesting technical problems I solved while developing this app, so stay tuned...