---
id: 385
title: Loading Tiled, Same-Name Data in Batch Mode.
date: 2011-02-28T12:30:43-06:00
author: Matt
layout: post
guid: http://maprantala.com/?p=385
permalink: /2011/02/28/loading-tiled-same-name-data-in-batch-mode/
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
  - ESRI
  - Python
tags:
  - arcgis 10
  - arcpy
  - ArcSDE
  - python
---
I have been loading existing raster data into a geodatabase to be included in a new Mosaic Dataset&#8211;a very cool and useful addition to ArcGIS 10. The most time-consuming part of the process for the human (at least this human) has been getting the names of the rasters right.

Our existing data is organized by tiles with the directory name representing the tile name and then the data within each tile directory having the same name.

For example:  
C:\GIS_dataAdamsparcels.shp  
C:\GIS_dataBuchetteparcels.shp

This makes batch loading the data less efficient because I end up having to rename the data or else end up with a series of feature classes named parcels, parcels\_2, parcels\_n.

So I hacked out a quick script that takes an input raster and figures out the final name I want it to have based of the directory name.

First, I used the Copy Raster (In ArcToolbox: Data Management-Raster-Raster Dataset-Copy Raster) and copied on sample to my geodatabase.

Then, I went to the Results Tab (Select Geoprocessing from the Menubar, Geoprocessing-Results) and right-clicked on the Copy Raster result and selected &#8220;Copy as Python Snippet&#8221;.

I then created a new python script and pasted the one line.

I added some imports, accepted a parameter, some string manipulation, and some result outputs and I had a quick & easy script. In added the script in ArcToolbox and now I can right-click on it and run it in Batch mode. I do a quick search in Windows Explorer to get all the rasters I want to run it on and select & drag them to my ArcToolbox Batch Dialog.

Actual code can be downloaded [HERE](http://dl.dropbox.com/u/22241283/NodeDangles/20110228_load_lidar_data.zip) and you don&#8217;t need to worry about WordPress messing up the spacing.

<pre>import arcpy
import os, sys

inRaster = sys.argv[1] 
basedir = os.path.basename(os.path.dirname(inRaster)).lower()
outRaster = "Database Connections/mgs_lidar.lidar.sde/mgs_lidar.lidar."+basedir

def printit(inMessage):
    print inMessage
    arcpy.AddMessage(inMessage)
    
if not (arcpy.Exists(outRaster)):
    printit ("Importing: "+basedir)
    arcpy.CopyRaster_management(inRaster,outRaster,"#","#","#","NONE","NONE","#")
else:
    printit ("Skipping: "+basedir+" because it already exists!")
</pre>

<div id="geo-post-385" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>