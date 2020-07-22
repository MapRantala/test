---
id: 298
title: Checking to see if a Field Index Exists Using Python (geoprocessing 9.3).
date: 2011-01-27T15:31:48-06:00
author: Matt
excerpt: Checking to see if a field index exists using Python geoprocessing in ArcGIS 9.3.
layout: post
guid: http://maprantala.com/?p=298
permalink: /2011/01/27/checking-to-see-if-a-field-index-exists-using-python/
geo_latitude:
  - "0"
geo_longitude:
  - "0"
geo_accuracy:
  - "0"
geo_public:
  - "1"
categories:
  - ArcScripts
  - Bugs
  - geoprocessing
  - Python
tags:
  - arcgisscripting
  - ArcSDE
  - exists
  - field
  - field index
  - geodatabase
  - index
  - python.geoprocessing
---
NOTE:  I have a [post here](http://maprantala.com/2011/02/21/checking-to-see-if-a-field-index-exists-using-arcpy-argis-10-0/) that shows how to check if a field exists using arcpy in ArcGIS 10.0.

In developing a python script to reload a geodatabase, I wanted to create any necessary indexes.

No problem creating the index, for example:

<pre>gp.AddIndex_management(tablename, field, IndexName, "NON_UNIQUE", "NON_ASCENDING")
</pre>

But before creating the index, I wanted to verify that it did not exist.  I tried the ever-popular, exists but could not get it to work&#8211;either it does not detect indexes or I just never got the fully-qualified name for the index right (ArcSDE using a postgres datastore).

<pre>gp.Exists(mgs_c5ix_fullname)</pre>

I finally found this [ArcGIS Desktop Help 9.3 &#8211; ListIndexes method](http://webhelp.esri.com/arcgiSDEsktop/9.3/index.cfm?TopicName=ListIndexes_method) from ESRI.  Unfortunately, it doesn&#8217;t work-it did not like the &#8220;while&#8221; loop construction.  I&#8217;m guessing it worked in 9.2 and despite ESRI&#8217;s own [warning](http://webhelp.esri.com/arcgiSDEsktop/9.3/index.cfm?TopicName=Differences_between_geoprocessor_versions) about differences in 9.2 & 9.3, they did not update the sample code.

A key is to make sure you create a 9.3-version geoprocessing object and the following code can be used.  The caveat that I need to include is that the code only checks one table, if the index is on a different table, it will give you a false-negative.

<pre>gp = arcgisscripting.create(9.3)

def indexExists(tablename,indexname):
 if not gp.Exists(tablename):
  return False

 indexList = gp.listindexes(tablename)

 for iIndex in indexList:
  if (iIndex.Name == indexname):
   return True

 return False
</pre>

To call it, just pass the table and indexname you are looking for.

<pre>indexExists(tablename,indexname)
</pre>

<div id="geo-post-298" class="geo geo-post" style="display: none">
  <span class="latitude"></span><span class="longitude"></span>
</div>