---
id: 715
title: 'Quick &#038; Dirty arcpy: Autopan ArcMap using arcpy'
date: 2012-05-02T05:19:40-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=715
permalink: /2012/05/02/quick-dirty-arcpy-autopan-arcmap-using-arcpy/
tagazine-media:
  - 'a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:8:"14087936";s:7:"blog_id";s:8:"13690265";s:9:"mod_stamp";s:19:"2012-04-27 14:55:26";}'
categories:
  - ArcGIS 10
  - arcpy
  - ArcScripts
  - Programming
  - Python
  - 'Quick &amp; Dirty'
tags:
  - arcgis
  - ArcMap
  - arcpy
---
Question: How do I get ArcMap to automatically pan through an area.

As I mentioned in a [previous post](http://maprantala.com/2012/04/30/building-a-loc…f-bing-imagery/), I recently had the need to have ArcMap automatically pan through a project area. My first attempt was to print a series of data-driven pages (using a fishnet polygon layer as the index) this but that did not accomplish what I needed so I switched to arcpy, which made the task simple enough. Nothing special or tricky about this code, but just did not find it anywhere else.

The one thing to note is that I have a 1 second pause between pans&#8211;this was to allow image tiles to download. You will need to adjust the delay to meet your needs. [The toolbox and code can also be downloaded](http://dl.dropbox.com/u/22241283/NodeDangles/20120427_AutoPan.zip).

<pre>import sys,arcpy,datetime
inLayer = sys.argv[1]

def printit(inMessage):
    print inMessage
    arcpy.AddMessage(inMessage)

mxd = arcpy.mapping.MapDocument("CURRENT")

arcpy.MakeFeatureLayer_management(inLayer, "indexLayer")
cur=arcpy.SearchCursor("indexLayer")

df = arcpy.mapping.ListDataFrames(mxd)[0]
newExtent = df.extent

iCount = 0
iTotal = (arcpy.GetCount_management("indexLayer").getOutput(0))

for row in cur:
    thisPoly = row.getValue("Shape")
    newExtent.XMin, newExtent.YMin = thisPoly.extent.XMin, thisPoly.extent.YMin
    newExtent.XMax, newExtent.YMax = thisPoly.extent.XMax, thisPoly.extent.YMax
    df.extent = newExtent
    time.sleep(1)
    iCount+=1
    printit("Panned to feature {0} of {1}".format(iCount,iTotal))

del row
del cur</pre>