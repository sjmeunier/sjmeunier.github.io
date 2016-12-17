---
layout: post
title:  "Ancestry Web 3D Web Demo"
date:   2016-12-17 13:00:03
categories: Programming
tags: [c-sharp, unity, genealogy, programming]
---

Last month, I [posted](https://sjmeunier.github.io/programming/2016/11/06/family-tree-visualisation-with-unity3d.html) about the family tree visualisation app I wrote using Unity. I have now made a lot of improvements, and, in addition, created a web demo as well. 

The web demo allows the app to be demonstrated from within a web browser without having to run it locally. The big disadvantage of the web demo, is that the Unity web player doesn't allow loading of external files, so it cannot be used to load any GEDCOM file, which the desktop application allows you to do.

However, to demonstrate the application, the demo comes pre-loaded with my ancestry.

To navigate through the scene, moving the mouse while left clicking pans the view, while right-clicking rotates the camera. The up and down arrow keys moves the camera forwards and backwards.

Hovering over one of the indivdual spheres brings up a detailed info pane for the person.
<!--more-->

To access the settings screen, press Esc. 

From the settings, you can control the root person, as well as drawing options, such as the depth to render. The higher the maximum depth, the more generations are shown, but that comes with the cost of longer processing time. The maximum number of generations in the demo data is 44 generations.

Each generation is normalised to fit on a circle, and evenly spaced. This is to help with space. While the number of potential individuals at a certain generations increases exponentially, in reality, there are not nearly so many known individuals at a certain generation. This is due to intermarriage amongst ancestors, as well as dead ends. This means there is a managable number of individuals at each generation, and can therefore be shown fairly easily. The size of the spheres also indicates the number of times that individual appears in the family tree, which indicates the number of lines of decent between the root person and that individual. The bigger the sphere, the more lines of decent.

One note: The Unity Webplayer does not work in Chrome. I have tested it in Microsoft Edge and Firefox which work fine.

The web demo is available at [https://sjmeunier.github.io/AncestryWeb3D/](https://sjmeunier.github.io/AncestryWeb3D/)

The full source code is available at [https://github.com/sjmeunier/ancestry-web-3d](https://github.com/sjmeunier/ancestry-web-3d).

![AncestryWeb3D](/assets/images/blog/ancestryweb3d.png){: .shadow-image .centered }

