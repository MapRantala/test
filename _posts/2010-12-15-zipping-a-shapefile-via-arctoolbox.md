---
id: 246
title: Zipping a shapefile via ArcToolbox
date: 2010-12-15T14:36:04-06:00
author: Matt
layout: post
guid: http://maprantala.com/?p=246
permalink: /2010/12/15/zipping-a-shapefile-via-arctoolbox/
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
twitter_cards_summary_img_size:
  - 'a:7:{i:0;i:530;i:1;i:140;i:2;i:2;i:3;s:24:"width="530" height="140"";s:4:"bits";i:8;s:8:"channels";i:3;s:4:"mime";s:10:"image/jpeg";}'
categories:
  - Misc
  - Python
tags:
  - arcgis
  - arcgisscripting
  - geoprocessing
  - python
  - shapefile
  - zip
---
## **UPDATE:** 

[After receiving a request to modify the code to ignore .lock files, I have an updated to this post.](http://maprantala.com/2014/05/06/zipping-a-shapefile-from-arccatalog/)

&nbsp;

I&#8217;ve received a request on how to use the [Zip Shapefile](http://maprantala.com/2010/12/10/zipping-a-shapefile-using-python/) code I posted last week from ArcGIS. Sorry, I did not set the code up to call directly from ArcGIS but only as an illustration of how it can be done.

I have, however, with some minor tweaking, made a version that can added to ArcToolbox. The steps to install are below, please note that at one point I had you download a \*.zip file that had been renamed to \*.jpg&#8211;this should now be corrected and the link should lead you directly to zipshapefile.zip.  Because of this steps two and three are obsolete.

1) Download the code from here.  
2) <del>Rename the file from zipshapefile-zip.jpg back to zipshapefile.zip.</del>  
3) Unzip the file.  
4) Move ZipShapefile.py to C:Program FilesArcGISArcToolBoxScriptsZipShapefile.py.  
5) Optionally, move Zip Shapefile.tbx, perhaps C:Program FilesArcGISArcToolBoxToolboxes.  
6) Add the toolbox to ArcToolbox. ESRI has instructions [here](http://webhelp.esri.com/arcgisdesktop/9.3/tutorials/spatial/Spatial_14.htm) on how to do this.

You should now have a new toolbox named &#8220;Zip Shapefile&#8221; with a script named &#8220;Zip a Shapefile&#8221; in it. Clicking on on the tool will bring up this dialog.

[<img class="alignnone size-medium wp-image-249" title="Zip a Shapefile Dialog" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2010/12/capture.jpg?resize=271%2C71" alt="" width="271" height="71" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2010/12/capture.jpg)

\***\***\***\***\***\***\***\***\***\***\****  
In response to Chris:

I believe you need to copy the ZipShapefile.py file from the .zip that you downloaded to C:Program FilesArcGISArcToolBoxScripts, the error message is consistent with the tool not being about to find the python script there.

If you prefer to place the ZipShapefile.py in a different location, you will need to change the source on the tool. To do this, right click on the tool in ArcCatalog and change the path of the Script File as set in the Source tab (see below):[<img class="aligncenter size-full wp-image-648" title="set Path" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2012/03/setpath.png?resize=494%2C265" alt="" width="494" height="265" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2012/03/setpath.png)

<div id="geo-post-246" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>