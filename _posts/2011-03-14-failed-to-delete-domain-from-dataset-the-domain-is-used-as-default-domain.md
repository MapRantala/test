---
id: 440
title: Using arcpy to List Domains Assigned to Featureclass Fields
date: 2011-03-14T18:04:36-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=440
permalink: /2011/03/14/failed-to-delete-domain-from-dataset-the-domain-is-used-as-default-domain/
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
  - ArcGIS
  - ArcGIS 10
  - ArcGIS 9.3.1
  - arcpy
  - ESRI
  - geoprocessing
  - Python
tags:
  - arc gis
  - arcgis 10
  - arcpy
  - coded-value domains
  - ESRI
  - geodatabase
  - gisscripting
  - python
---
I was making an edit (adding leading &#8220;0&#8221;s) to a coded-value domain in an SDE database and realized that my edits were changing the order of the rows of my domain.  Rows were moved to the bottom of the list when they were edited.

So I went through the process of converting my domain back to a table, made my edits in Access and exported the rows to a .dbf in the order I wanted them.

I removed the domain from the field I knew it was assigned to but when I tried to delete the domain, I received an error (The domain is used as a default domain).

[<img class="aligncenter size-full wp-image-443" title="The domain is used as a default domain." alt="" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/03/failedtodeletedomain.png?resize=532%2C240" width="532" height="240" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/03/failedtodeletedomain.png)

The Google Machine led me to an [ArcForums post](http://forums.esri.com/Thread.asp?c=93&f=985&t=303616) by  with some python code for listing all the fields with a domain.

I tweaked the original a bit, first because it was only examining feature classes in a feature dataset, not stand-alone feature datasets.  And secondly, I updated it to use arcpy.  I posted both the [9.3 version](http://dl.dropbox.com/u/22241283/NodeDangles/20110314_ListDomains-9_3.zip) and the [10.0 version](http://dl.dropbox.com/u/22241283/NodeDangles/20110314_ListDomains-10.zip), but here is a quick look.  The key addition is the &#8216;listfc(&#8220;&#8221;)&#8217; line that is the first line of the def listds() module.

<pre>import arcpy

min_workspace = r"C:\Users\mranter\AppData\Roaming\ESRI\Desktop10.0\ArcCatalog\min.minstaff.sde"

infgdb=(min_workspace)
arcpy.env.workspace = infgdb

def listfc(inDataset):
   featureclasses = arcpy.ListFeatureClasses("","",inDataset)
   for f in featureclasses:
      print "feature class: ",f

lfields=arcpy.ListFields(f)

for lf in lfields:
   if lf.domain &lt; "":
      print "      domain",f, lf.name, lf.domain

def listds():
   listfc("")

   datasets=arcpy.ListDatasets ("","")
   for d in datasets:
      print "  dataset: ",d

listfc(d)
listds()</pre>

<div id="geo-post-440" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>