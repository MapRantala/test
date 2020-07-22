---
id: 546
title: Renaming Raster Dataset and arcpy.Exists()
date: 2011-05-03T09:33:48-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=546
permalink: /2011/05/03/renaming-raster-dataset-and-arcpy-exists/
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
  - ArcMap
  - ArcObjects
  - arcpy
  - ArcScripts
  - ArcSDE
  - ESRI
  - geoprocessing
  - PostgreSQL
  - Python
tags:
  - arcgis
  - arcgis 10
  - ArcObjects
  - arcpy
  - ArcScripts
  - ArcSDE
  - ESRI
  - GIS
  - python
---
Discovered something today. I was working on an arcpy script that copies a raster dataset from a file geodatabase into a Postgres SDE geodatabase and then does some boring routine tasks&#8211;building stats, creating a mosaic dataset, adding the raster to the mosaic dataset and making a couple referenced mosaic datasets.

It sometimes has trouble with the initial step of uploading the raster because of the sheer size of if (1m elevation raster for counties) and it failed today on one. It failed today so I used the ArcCatalog GUI to copy the raster and renamed it.

I then proceeded to run launch my script. Before each step, I use [arcpy.Exists](http://help.arcgis.com/en/arcgisdesktop/10.0/help/index.html#/Exists/000v00000021000000/)() extensively to check to see if various items exist before I attempt to create them. It was continuously reporting that my raster set did not exist even though I could see it in ArcCatalog.

Finally, I realized that I needed to close ArcCatalog before arcpy recognized the fact I had renamed something. To note, I was running arcpy from a separate PythonWin window, not from the ArcCatalog session I had renamed the raster dataset with.

Once I closed ArcCatalog, arcpy recognized the renaming and life was good.

I&#8217;m also suspicious now about a problem I often have running statistics on my rasters.  The ArcTool reports no errors when I create them but for some reason the raster does not show that it has statistics afterwards.  I normally have multiple ArcApplication sessions open and now suspect that perhaps this problem is due to sessions not letting go of the connection.  Stay tuned for further developments on this.

<div id="geo-post-546" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>