---
id: 508
title: Change Detector arcpy Script
date: 2011-04-08T10:48:59-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=508
permalink: /2011/04/08/change-detector-script-arcgis-resource-center/
geo_longitude:
  - "-93.55073"
geo_latitude:
  - "44.852994"
geo_accuracy:
  - "1447"
geo_address:
  - 8341-8345 Suffolk Dr, Chanhassen, MN 55317, USA
geo_public:
  - "1"
categories:
  - ArcGIS 10
  - arcpy
  - ArcScripts
  - ESRI
  - geoprocessing
  - Python
tags:
  - arcgis
  - arcgis 10
  - ArcMap
  - arcpy
  - ArcScripts
  - detecting changes
  - feature class
  - geodatabase
  - python
  - shapefile
---
During a process I was working on, I needed to compare a feature class before and after some edits.  I did not quickly find anything in ArcToolbox but searching [ArcResources](http://resources.arcgis.com/) led me to [Change Detector script by Bruce Harold](http://resources.arcgis.com/gallery/file/Geoprocessing-Model-and-Script-Tool-Gallery/details?entryID=351BEE10-1422-2418-8815-82074A3E6B6C).  After making a couple of tweaks&#8211;for some reason in one of my feature classes, the Shape field had an upper case &#8220;S&#8221; and in the other it was a lower case &#8220;s&#8221;.  I also discovered that it needs to export to the same format (personal geodatabase, file geodatabase, shapefile) as the source data (or at least one that uses the same field name deliminator).

After minor adjustments, though, it worked like a charm.  I&#8217;ll be submitting the changes I made to Bruce and let him incorporate the changes into the official code.

FOLLOW-UP: Mr. Harold quickly responded to my email & made the change (although I haven&#8217;t checked it). Way to go Bruce!  Thanks for a handy script.

<div id="geo-post-508" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>