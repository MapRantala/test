---
id: 364
title: Checking to see if a Field Index Exists Using Arcpy (ArGIS 10.0)
date: 2011-02-21T09:55:35-06:00
author: Matt
excerpt: Modifying a geoprocessing 9.3 python script to an arcpy 10.0 python script requires a modification on how to check if a table has a field index.
layout: post
guid: http://maprantala.com/?p=364
permalink: /2011/02/21/checking-to-see-if-a-field-index-exists-using-arcpy-argis-10-0/
geo_latitude:
  - "44.852994"
geo_longitude:
  - "-93.55073"
geo_accuracy:
  - "1447"
geo_address:
  - 8341-8345 Suffolk Dr, Chanhassen, MN 55317, USA
geo_public:
  - "1"
categories:
  - ArcGIS 10
  - arcpy
tags:
  - arcgis 10
  - arcpy
  - check index exists
  - index exists
  - python
---
Updating some python code from 9.3 that using geoprocessing  to 10.0 using arcpy and the first real object I&#8217;ve had to change relates to detecting whether or not an index exists on a table.

I previously [posted code](http://maprantala.com/2011/01/27/checking-to-see-if-a-field-index-exists-using-python/) using a 9.3 geoprocessing commands, the core of it being:

<pre>indexList = gp.listindexes(tablename)
for iIndex in indexList:
    if (iIndex.Name == indexname):
       return True
return False
</pre>

With arcpy, ESRI has gone back to using the [Describe](http://help.arcgis.com/en/arcgisdesktop/10.0/help/index.html#/Describe/000v00000026000000/) methodology.  Way back when GISers use to type most of their commands into their wood-burning computers, the command &#8220;Describe&#8221; was used to retrieve information about a coverage, grid, or other data set in our fancy AMLs (or SMLs).  The snippet below shows a function for checking to see if a table has an index with a specified named.

<pre>def indexExists(tablename,indexname):

 if not arcpy.Exists(tablename):
  return False

 tabledescription = arcpy.Describe(tablename)

 for iIndex in tabledescription.indexes:
  if (iIndex.Name == indexname):
   return True

 return False
</pre>

<div id="geo-post-364" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>