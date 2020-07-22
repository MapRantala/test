---
id: 697
title: 'ArcMap Field Calculator: Identifying Unique Cases, Single Field'
date: 2012-04-28T05:55:34-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=697
permalink: /2012/04/28/arcmap-field-calculator-identifying-unique-cases-single-field/
categories:
  - ArcGIS 10
  - ArcMap
  - arcpy
  - ESRI
  - Field Calculator
tags:
  - arcgis
  - ArcMap
  - arcpy
  - Field Calculator
  - Unique Case
---
Seems like a lot of people are finding the ArcMap Field Calculator [examples](http://maprantala.com/category/esri/arcmap-esri/field-calculator/) that I have posted useful so I will make an effort to post more of them. Most posts are generated after I do something and think that others might want to know how to do it. (Or so I can go back and remember how I did something without re-inventing it).

Something I did today was create a field (!Case!) and then populated this with a unique identifier for each different value (case) that occurred in a different field (!Feature!).

**Note: python&#8217;s _index_ statement is a 0-based search so the first case will have the value 0, the second will have 1, and so on. If you want to start the results at 1, you can make the last line: &#8220;return caseList.index(inValue) + 1&#8221;.**

The basic structure for this is shown:

<pre>caseList = [ ]

def returnCase(inValue):
   global caseList

   if not inValue in caseList:
      caseList.append(inValue)

   return caseList.index(inValue)</pre>

<img class="aligncenter size-full wp-image-700" title="Case" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2012/04/case.png?resize=614%2C449" alt="" width="614" height="449" data-recalc-dims="1" />