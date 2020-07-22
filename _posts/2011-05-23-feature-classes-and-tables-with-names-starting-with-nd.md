---
id: 564
title: 'Feature classes and Tables with names starting with &#8220;nd_&#8221;.'
date: 2011-05-23T15:55:24-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=564
permalink: /2011/05/23/feature-classes-and-tables-with-names-starting-with-nd_/
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
  - ArcSDE
  - Bugs
  - ESRI
  - PostgreSQL
  - Python
tags:
  - ArcCatalog
  - arcgis
  - ArcSDE
  - Bugs
  - ESRI
  - geodatabase
  - python
---
Random luck me to discovering a bug related to feature classes whose names start with &#8220;nd\_&#8221;.  It appears that you are allowed to create feature classes starting with &#8220;nd\_&#8221; but ArcCatalog will not display them.  Further research shows this behavior also occurs for table and for ArcSDE (PostGres) geodatabases,  personal geodatabase, and file geodatabases&#8211;I am using ArcCatalog 10.0.

I first noticed something odd was occurring while importing a series of shapefiles into a geodatabases.  After importing 15 shapefiles, I only had thirteen feature classes despite receiving no errors during the process.  The two shapefiles that failed to import were named ND\_oil\_and\_gas.shp and ND\_Bendix_Study.shp.  Subsequent attempts to import them individually returned an error &#8220;Invalid Target Name&#8221;.

[<img class="aligncenter size-full wp-image-565" title="Invalid Target Name" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/05/invalidtargetname.png?resize=237%2C186" alt="" width="237" height="186" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/05/invalidtargetname.png)

I discovered in pgAdmin III (Postgres SDE Geodatabase) that the table existed and there was an entry in sde.sde_layers for the feature class but ArcCatalog refused to show it.

[<img class="aligncenter size-full wp-image-570" title="nd_" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2011/05/nd_2.png?resize=630%2C385" alt="" width="630" height="385" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2011/05/nd_2.png)

I used some un-supported methods to try to resolve the problem and despite some sweating, I failed to find a way to get ArcCatalog to display these feature classes.  I did, however, at least found a way to delete them&#8211;arcpy can detect that the feature classes exists so it is able to delete them.

[<img class="aligncenter size-full wp-image-571" title="arcpy.delete_management" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2011/05/arcpy_delete.png?resize=630%2C129" alt="" width="630" height="129" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2011/05/arcpy_delete.png)

At least by deleting them, I can prevent leaving &#8220;invisible&#8221; feature classes from hanging out in my geodatabase.

I suspect the problems stems from how ESRI has implemented the [Network dataset table-naming structure](http://help.arcgis.com/en/arcgisdesktop/10.0/help/index.html#//002p0000007m000000.htm) &#8211;dirty areas are stored in tables named _nd\_<itemid>\_dirtyareas _ and _nd\_<itemid>\_dirtyobjects_.  Possibly the developer  working on the ArcCatalog GUI ended up suppressing showing feature classes and tables whose names start with &#8220;nd_&#8221;.

And, just for posterity&#8217;s sake, here is a python code snippet listing the feature classes in a workspace:

> import arcpy
> 
> arcpy.env.workspace = &#8220;c:/temp/_nd/F.gdb&#8221;
> 
> print arcpy.env.workspace  
> for fc in arcpy.ListFeatureClasses():  
> print fc
> 
> print &#8220;Done!&#8221;

<div id="geo-post-564" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>