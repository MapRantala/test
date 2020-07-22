---
id: 312
title: Zipping a File Geodatabase using Python
date: 2011-02-08T13:44:34-06:00
author: Matt
layout: post
guid: http://maprantala.com/?p=312
permalink: /2011/02/08/zipping-a-file-geodatabase-using-python/
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
  - ArcScripts
  - ESRI
  - geoprocessing
  - Python
tags:
  - compressing
  - file geodatabase
  - python
  - shapefile
  - zipping
  - zipping a file geodatabase
---
Ever since the ever-popular post, [Zipping a shapefile using python](http://maprantala.com/2010/12/10/zipping-a-shapefile-using-python/), came out, people have been asking (one person, yesterday) for a sample of how to zip a file geodatabase using python.

The key trick, as shown in line 17, is appending the basename of the file geodatabase (&#8220;nfg.gdb/&#8221; in my example) in front of each file as you write it to the zipfile.

UPDATE: WordPress messes with the spacing when I post code, making it difficult to post code that can just be copied & pasted and have work.  So I have posted a the code [HERE](http://dl.dropbox.com/u/22241283/NodeDangles/20110208_Sample_Zip_FileGeodatabase.zip) for downloading.

<pre>import os
import zipfile
import glob

infile = "c:/temp/nfg.gdb"
outfile = "c:/temp/nfg.zip"
def zipFileGeodatabase(inFileGeodatabase, newZipFN):
   if not (os.path.exists(inFileGeodatabase)):
      return False

   if (os.path.exists(newZipFN)):
      os.remove(newZipFN)

   zipobj = zipfile.ZipFile(newZipFN,'w')

   for infile in glob.glob(inFileGeodatabase+"/*"):
      zipobj.write(infile, os.path.basename(inFileGeodatabase)+"/"+os.path.basename(infile), zipfile.ZIP_DEFLATED)
      print ("Zipping: "+infile)

   zipobj.close()

   return True

zipFileGeodatabase(infile,outfile)

</pre>

<div id="geo-post-312" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>