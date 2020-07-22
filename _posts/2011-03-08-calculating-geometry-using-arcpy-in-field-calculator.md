---
id: 409
title: 'ArcMap Field Calculator: Calculating Geometry using arcpy'
date: 2011-03-08T09:30:40-06:00
author: Matt
excerpt: 'Simple examples of calculating the [Shape] of records using the Field Calculator in ArcGIS 10'
layout: post
guid: http://maprantala.com/?p=409
permalink: /2011/03/08/calculating-geometry-using-arcpy-in-field-calculator/
categories:
  - ArcGIS 10
  - arcpy
  - ESRI
  - Field Calculator
tags:
  - Arc
  - arcgis
  - arcgis 10
  - ArcMap
  - arcpy
  - ESRI
  - Field Calculator
  - geometry
  - GIS
  - python
---
One of the things I had not gotten around to doing in ArcGIS 10 was figure out how to directly manipulate the geometry of a record using the [Field Calculator](http://help.arcgis.com/en/arcgisdesktop/10.0/help/index.html#//005s00000025000000.htm).  When I stumbled upon a bug in the way the [Extract Values to Points](http://maprantala.com/2011/03/07/extract-values-to-points-spatial-analyst-bug/) tool handles Null geometries, I figured it was time to figure it out.  Setting the X, Y to 0,0 was sufficient for my needs.

I set the Parser to Python and the formula was simple once I figured out the syntax:

<pre>pPoint = arcpy.Point(0,0)</pre>

[<img class="aligncenter size-full wp-image-412" title="Use Field Calculator to Calculate Geometry" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2011/03/origin.jpg?resize=479%2C576" alt="" width="479" height="576" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2011/03/origin.jpg)

Then, to extend my knowledge, I wanted to see how to calculate the geometry based off of two fields (X and Y).   The only real trick is knowing the bracket field names using exclamation points instead of brackets:

<pre>arcpy.Point(!X!,!Y!)</pre>

[<img class="aligncenter size-full wp-image-413" title="Using Field Calculating to Calculate Geometry Using Attributes" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2011/03/fields.jpg?resize=630%2C422" alt="" width="630" height="422" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2011/03/fields.jpg)

So turns out everything it pretty easy and straight-forward.  Whew!