---
id: 4497
title: 'Quick &#038; Dirty ArcPy: Listing Data Sources'
date: 2014-12-03T08:05:10-06:00
author: Matt
layout: post
guid: http://maprantala.com/?p=4497
permalink: /2014/12/03/quick-dirty-arcpy-listing-data-sources/
categories:
  - arcpy
  - Python
  - 'Quick &amp; Dirty'
tags:
  - arcgis
  - arcpy
  - ESRI
  - GIS
  - python
---
I just had the need to go through a directory containing many (100+) layer files (.lyr) and verify the data sources in each. I could have loaded each into ArcMap and checked the properties, but choose not to. Here&#8217;s the [bare-bones script](https://github.com/MapRantala/Blog/blob/master/python/arcpy/20141203_ListDataSources/listDataSources.py) I used instead:

<pre>import arcpy, glob,os

theDir = r"L:\gdrs\data\org\us_mn_state_dnr\elev_minnesota_lidar\\"
os.chdir(theDir)

for iFile in glob.glob("*.lyr"):
    print iFile
    lyr = arcpy.mapping.Layer(iFile)
    for i in arcpy.mapping.ListLayers(lyr):
        try:
            print "    {0}: {1}".format(i,i.dataSource)
        except:
            print "    {0}: Does not support dataSource".format(i)

print "Done!"

</pre>