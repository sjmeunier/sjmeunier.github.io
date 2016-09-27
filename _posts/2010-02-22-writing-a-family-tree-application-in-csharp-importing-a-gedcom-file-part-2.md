---
layout: post
title:  "Writing a Family Tree Application in C#: Importing a Gedcom File - Part 2"
date:   2010-02-22 00:00:03
categories: Programming
tags: [genealogy, c-sharp, tutorials]
---

Now that we know how gedcom files work (covered in [part 1](/programming/2010/02/22/writing-a-family-tree-application-in-csharp-importing-a-gedcom-file-part-1.html)), we need to actually parse the file.

The way in which the gedcom is structured is that each new record entry starts on a line with a 0. All lines with a 1 under that belong to that record. Similarly, lines with a 2 prefix, belond to the the entry prefixed by a 1 directly above it.

This makes processing rather straightforward. All we need to do, is read the file line by line, then split up the line into its components, and process accordingly.

All lines have a number (0, 1 or 2) as the first field in the line, and each field is seperated by a space, for example 1 SOUR FamilyTraces

The second field in the line is usually the keyword which tells us what the data represents. Some keywords will not need an extra data field in the third field, but usually, these types of lines which are grouped together with that line.
As an example, the VERS and FORM lines are linked to the GEDC keyword as the 2 in the first field shows they belong to the preceding line.

{% highlight text %}
1 GEDC
2 VERS 5.5
2 FORM LINEAGE-LINKED
{% endhighlight %}
<!--more-->

There is an exception to the second field being the keyword, and that is in the individual, family and note record entry fields which take the format 0 @Ixxx@ INDI for an individual record for example.

Based on all of that, all we need to do, is split up the line into the three fields, and then process each keyword independantly from each other, while keeping track of which type of record we are currently in, so that we can assign the data to the correct structure.

[Here](https://github.com/sjmeunier/family-traces) is the full code for the parser, that works pretty much exactly as described above. I have not implemented the full spec of the gedcom file format, but only enough as I need for the application. It is easily modified, however, to add the rest of the spec in if needed.

Usually, now would be the time where I list the source code for the code I have talked about in this blog post, but due to some bug with WordPress and the plug-in I am using to render my code samples nicely, the code for this section is crashing Firefox, so I have included the code for the GedcomParser class as a link to a text file containing the code.

_Originally posted on my old blog, Smoky Cogs on 22 Feb 2010_