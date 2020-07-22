---
id: 4774
title: Using ArcPy.Mapping to update Definition Queries.
date: 2018-05-08T17:56:09-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=4774
permalink: /2018/05/08/using-arcpy-mapping-to-update-definition-queries/
categories:
  - ArcMap
  - arcpy
  - arcpy.mapping
  - Python
  - 'Quick &amp; Dirty'
tags:
  - ArcMap
  - arcpy
  - ESRI
  - python
---
Working with a routine process today that I normally only do once in awhile but today needed to do it several times. It requires changing the definition query on several features classes. Being the &#8220;lazy&#8221; GIS guy that I am (owner of a company I used to work at called me that once as a sort of compliment for my tendency to script a lot of what I did), I decided to finally script it instead of changing definition queries about 42 times.

The quick & dirty <del>script</del> code that I wrote & can be run in the ArcMap python window consists of two parts. The first three lines just need to be run once per session to get a variable set with the list of layers. The loop I ran multiple times, updating the &#8220;.replace&#8221; settings each time. I ran the loop, did my other processes, and re-ran the loop as needed and was a happy GISer.

<pre>#These first 3 lines only need to be run once.
mxd = arcpy.mapping.MapDocument("CURRENT")
l = arcpy.mapping.ListDataFrames(mxd)[0] #Note the hard-code [0], meaning the first dataframe. YMMV
ll = arcpy.mapping.ListLayers(l)

#Everytime I wanted to update the definition queries just need to update the replace parameters.

for l in ll:
     try:
        print(l.name)
        print(l.definitionQuery)
        z = l.definitionQuery.replace("_011","_012")
        l.definitionQuery = z
     except:
        print("")
</pre>