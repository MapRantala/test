---
id: 125
title: Michigan Office of Geological Survey
date: 2010-07-22T15:31:20-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=125
permalink: /2010/07/22/michigan-office-of-geological-survey/
geo_latitude:
  - "0"
geo_longitude:
  - "0"
geo_accuracy:
  - "0"
geo_public:
  - "1"
categories:
  - ArcMap
  - ESRI
  - Geological Survey
  - State Geological Survey
tags:
  - Geological GIS data
  - Geology
  - GIS
  - Michigan
  - Michigan Office of Geological Survey
---
The Michigan [Office of Geological Survey](http://www.michigan.gov/deq/0,1607,7-135-3306_57064-87386--,00.html) appears to have pdf versions of all the documents in their [Digital Geological Library](http://www.michigan.gov/documents/deq/GIMDL-Catalog-2010-01-20_307979_7.pdf) available for download.  The transcripts of some early (beginning in 1871) [field notes](http://www.michigan.gov/documents/deq/OFR_19_Rominger_1871-1874_Aopt_306084_7.pdf) are a fun inclusion in the available archives.

Actual GIS data was a bit hard to find although I found both bedrock geology and quarternary geology available from the state [Geographic Data Library](http://www.mcgi.state.mi.us/mgdl/?action=thm) in shapefile format.  I also found oil and gas well data but did not download it.

The data carried minimal attribution, one interesting thing I found in the bedrock data was the red, green, blue values apparently used for each polygon.

One thing to be aware of is that the shapefiles do not come with a prj file which could be a problem.  Michigan has their own defined coordinate system called [Michigan GeoRef](http://www.michigan.gov/documents/DNR_Map_Proj_and_MI_Georef_Info_20889_7.pdf).  This coordinate system defines the entire state into a single zone instead of multiple zones like the USGS&#8217; stateplane system does.  This is nice to work with UNLESS it comes undocumented as I found out while doing some consulting work in Au Train, Michigan a decade ago&#8211;I was able to finally find something online about it around 2 am one morning but I think I still have scars from the head-pounding that occurred that night.

ESRI, however, makes it easy to deal with this data&#8211;they include the specifications for Michigan GeoRef in inventory of predefined coordinate systems that ships with ArcGIS.  If you define the projection on each shapefile, it will then load automatically in real-world coordinates.  To define the projection on a shapefile, right-click on it in ArcCatalog and select &#8220;Properties&#8230;&#8221;.  Select &#8220;XY Coordinate System&#8221; on the Shapefile Properties dialog.  Hit the &#8220;Select&#8221; button and navigate to Projected Coordinate Systems-State Sytems-NAD 1983 Michigan GeoRef (Meters).prj.  Hit the &#8220;Add&#8221; button and then the &#8220;Apply&#8221; button.  Repeat for any other shapefiles you download.

One item I found out about ArcGIS while doing this is that if you change the projection on a shapefile, it will recognize that change the next time you use that data.  I originally set the units to be feet on the data so it did not load in the proper real-world location.  I saved my ArcMap document, quit out, and corrected the problem.  When I re-opened the document, Michigan had migrated back to where we are used to seeing it.  This contrasts with how ArcView 3.x use to handle raster data, you had to actually remove the layer from your view and re-load it to recognize the change in spatial references.

<div id="geo-post-125" class="geo geo-post" style="display: none">
  <span class="latitude"></span><span class="longitude"></span>
</div>