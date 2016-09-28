---
layout: post
title:  "Writing a Family Tree Application in C#: Importing a Gedcom File - Part 1"
date:   2010-02-22 00:00:02
categories: Programming
tags: [genealogy, c-sharp, tutorials, programming]
---

In my previous post in this series, I introduced the Family Traces family tree application. Now, I am going to go through the importing of a gedcom file.

Firstly, what is a gedcom file? The gedcom file format is a text file format for the transmission and storage of genealogical data, created by the Church of the Latter Day Saints for their genealogical projects.

A standard gedcom file has a header section, individual records, family records and notes. The various records are linked to each other by ids, in the format of @I1@ for an individual record for example. For a full spec of the gedcom format, have a look here.

Here is a sample gedcom file
{% highlight text %}
0 HEAD
1 SOUR FamilyTraces
2 NAME Family Traces
2 CORP Serge Meunier
1 DEST Standard GEDCOM
1 DATE 2010/02/16
1 CHAR ANSEL
1 GEDC
2 VERS 5.5
2 FORM LINEAGE-LINKED
0 @I1@ INDI
1 John/Smith/
2 GIVN John
2 SURN Smith
1 SEX M
1 BIRT
2 DATE 15 dec 1950
2 PLAC
1 FAMC @F2@
1 FAMS @F1@
0 @I2@ INDI
1 Jane/Doe/
2 GIVN Jane
2 SURN Doe
1 SEX F
1 FAMC @F-1@
0 @F1@ FAM
1 HUSB @I1@
1 WIFE @I2@
1 CHIL @F3@
0 TRLR
{% endhighlight %}

Before we start processing the file, we need to have some data structures to hold the data from the file.
<!--more-->

{% highlight csharp %}
public struct GedcomHeader  
{  
    public string Source;  
    public string SourceVersion;  
    public string SourceName;  
    public string SourceCorporation;  
    public string Destination;  
    public string Date;  
    public string File;  
    public string CharacterEncoding;  
    public string GedcomVersion;  
    public string GedcomForm;  
}  
  
public struct GedcomIndividual  
{  
    public string Id;  
    public string GivenName;  
    public string Surname;  
    public string Suffix;  
    public string Sex;  
    public string BirthDate;  
    public string BirthPlace;  
    public string Occupation;  
    public string Description;  
    public string Nationality;  
    public string DiedDate;  
    public string DiedPlace;  
    public string DiedCause;  
    public string ParentFamilyId;  
    public string SpouseFamilyId;  
    public ArrayList Notes;  
  
    public GedcomIndividual(string id)  
    {  
        Id = id;  
        GivenName = "";  
        Surname = "";  
        Suffix = "";  
        Sex = "";  
        BirthDate = "";  
        BirthPlace = "";  
        Occupation = "";  
        Description = "";  
        Nationality = "";  
        DiedDate = "";  
        DiedPlace = "";  
        DiedCause = "";  
        ParentFamilyId = "";  
        SpouseFamilyId = "";  
        Notes = new ArrayList();  
    }  
}  
  
public struct GedcomFamily  
{  
    public string Id;  
    public string HusbandId;  
    public string WifeId;  
    public string MarriageDate;  
    public string MarriagePlace;  
    public ArrayList Children;  
    public ArrayList Notes;  
  
    public GedcomFamily(string id)  
    {  
        Id = id;  
        HusbandId = "";  
        WifeId = "";  
        MarriageDate = "";  
        MarriagePlace = "";  
        Children = new ArrayList();  
        Notes = new ArrayList();  
    }  
}  
  
public struct GedcomNote  
{  
    public string Id;  
    public string Text;  
  
    public GedcomNote(string id)  
    {  
        Id = id;  
        Text = "";  
    }  
}  
{% endhighlight %}

We also need a set of enumerations we will use to keep track of where we are in the file. This is important because the meaning of a field in the file is dependant on the place in the file it occurs, so we need to be aware of what we need to do at all times, but more on that later.

{% highlight csharp %}
public enum GedcomRecordEnum  
{  
    None,  
    Header,  
    Individual,  
    Family,  
    Note  
}  
  
public enum GedcomSubRecordEnum  
{  
    None,  
    HeaderSource,  
    HeaderGedcom,  
    IndividualName,  
    IndividualBirth,  
    IndividualDeath,  
    FamilyChildren,  
    FamilyMarriage  
}  
{% endhighlight %}

The full source is available at [https://github.com/sjmeunier/family-traces](https://github.com/sjmeunier/family-traces).

Now we need to get to parsing the file, which is covered in [Importing a Gedcom file – Part 2](/programming/2010/02/22/writing-a-family-tree-application-in-csharp-importing-a-gedcom-file-part-2.html)

_Originally posted on my old blog, Smoky Cogs on 22 Feb 2010_