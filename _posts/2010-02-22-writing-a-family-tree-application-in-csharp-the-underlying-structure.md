---
layout: post
title:  "Writing a Family Tree Application in C#: The Underlying Structure"
date:   2010-02-22 00:00:01
categories: Programming
tags: [genealogy, c-sharp, tutorials]
---

I love being able to trace my family tree, and have used several good genealogy applications which I have found very useful, including Family Tree Legends and Family Tree Builder. The problem I have though, is that they do not always contain everything which I am looking for, so I decided to write a family tree application to plug in the holes.

Family Traces, which you can download the full source for here, is a fully functional, albeit bare-boned family tree application, which can import and export Gedcom files, calculate ancestors and descendants, and allow you to edit the family tree. Everything is there for a starting point, from where you can develop further reports and functionality.

All of that will come in later posts in this series though. For now, I am going to focus on the core of the application – the data layout. This layout is created within an Access database included with the application.

The two main components of any family tree are individuals and families.

Starting with the individual, each person in the family tree will have a record in the database. The most important data we need for an individual are:

* Individual Id
* Name
* Birth date
* Birth place
* Death date
* Death place

These fields are vital in determining if the person we are looking at is the same person listed in genealogical records, such as birth certificates, genealogical lists etc.

Now data collection for an individual is not restricted to these fields, and you are free to create as many fields as your want for things like physical attributes, cause of death, photos and general notes.

Moving onto families now, a family is what connects different individuals together into a tree.

The main family record will have

* Family Id
* Husband Id
* Wife Id
* Marriage date
* Marriage place

This links the husband and wife individual records into the family, and allows you to track the marriage date and place, which is also important in genealogical research, as it may provide crucial hints to the individuals, particularly if the birth and death dates of the individuals are unsure.

The last thing we need is to be able to link individuals as children within a family, and this is done using a separate table, which keeps track of the family id, as well as the individual id of the child, and each family can have multiple individuals attached to it as children.

In the database setup for the application, I also included a field in the individual record for the parent family record, which makes it easier to link the individual to a family as a child when processing the tree. This means that we are able to find the family that the individual is a child of without having to check the family records directly.

Now that is all there is to the core of a genealogy application, so feel free to play around with the application, and I will explain more about how the gedcom file format works in the next tutorial…

_Originally posted on my old blog, Smoky Cogs on 22 Feb 2010_