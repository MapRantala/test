---
id: 236
title: Zipping a shapefile using python
date: 2010-12-10T18:20:13-06:00
author: Matt
layout: post
guid: http://maprantala.com/?p=236
permalink: /2010/12/10/zipping-a-shapefile-using-python/
geo_latitude:
  - "44.979965"
geo_longitude:
  - "-93.263836"
geo_accuracy:
  - "42000"
geo_address:
  - S Washington Ave, Minneapolis, MN 55415
geo_public:
  - "1"
categories:
  - ArcScripts
  - Python
tags:
  - arcobject
  - ESRI
  - python
  - shapefile
  - zip
  - zipping
---
## **UPDATE:** 

[After receiving a request to modify the code to ignore .lock files, I have an updated to this post.](http://maprantala.com/2014/05/06/zipping-a-shapefile-from-arccatalog/)

One of the tasks I&#8217;ve been automating is publishing a weekly data update to a website. The update consists of shapefile. The trouble with shapefiles is they consist of 3 or more files with the same basename but different extensions in the same directory.

Not an overly complicated situation but a common one that ArcGIS does not have a solution out-of-the-box. Below is a bare-bones code snippet to do it. It has both the input shapefile and output zip file hard-coded. The call to the subroutine that does the work is: zipShapefile(wellsShapeFile,wellsZipFile)

<pre>import zipfile
import sys
import os
import glob
wellsShapeFile = "C:/cwi5_bk/WELLS/wells.SHP"
wellsZipFile = "C:/cwi5_bk/WELLS/test5.zip"

def zipShapefile(inShapefile, newZipFN):
   print 'Starting to Zip '+inShapefile+' to '+newZipFN

if not (os.path.exists(inShapefile)):
   print inShapefile + ' Does Not Exist'
   return False

if (os.path.exists(newZipFN)):
   print 'Deleting '+newZipFN
   os.remove(newZipFN)

if (os.path.exists(newZipFN)):
   print 'Unable to Delete'+newZipFN
   return False

zipobj = zipfile.ZipFile(newZipFN,'w')

for infile in glob.glob( inShapefile.lower().replace(".shp",".*")):
   print infile
   zipobj.write(infile,os.path.basename(infile),zipfile.ZIP_DEFLATED)

zipobj.close()
return True

zipShapefile(wellsShapeFile,wellsZipFile)
print "done!"
</pre>

<div id="geo-post-236" class="geo geo-post" style="display: none">
  <span class="latitude">44.979965</span><span class="longitude">-93.263836</span>
</div>