---
id: 639
title: 'ArcMap Field Calculator: ArcPy Date to Decimal Function'
date: 2011-10-06T21:08:35-05:00
author: Matt
excerpt: arcpy function to convert a date to decimal value
layout: post
guid: http://maprantala.com/?p=639
permalink: /2011/10/06/field-calculator-arcpy-date-to-decimal-function/
categories:
  - ArcGIS 10
  - ArcMap
  - arcpy
  - Field Calculator
  - Python
tags:
  - arcgis 10
  - ArcMap
  - arcpy
  - date
---
One of the standards in our databases is to store dates as 8-digit integer values in the format of yyyymmdd. This requires us to occasionally convert values from date fields into this format.

We can do this in the ArcMap Field Calculator using this arcpy function:

<pre>def datetodouble(inNum):
     splitList = str(inNum).split("/")
     return  splitList [2]  +("0"+ splitList [0])[-2:]  +("0"+ splitList [1])[-2:]</pre>

[<img class="aligncenter size-full wp-image-640" title="Date To Double" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/10/datetodouble.png?resize=492%2C591" alt="" width="492" height="591" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/10/datetodouble.png)