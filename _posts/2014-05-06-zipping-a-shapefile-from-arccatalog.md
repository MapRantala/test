---
id: 943
title: Zipping a Shapefile from ArcCatalog
date: 2014-05-06T05:42:04-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=943
permalink: /2014/05/06/zipping-a-shapefile-from-arccatalog/
categories:
  - ArcGIS
  - ArcGIS 9.3.1
  - arcpy
  - Python
---
Back in 2010, I posted a <a href="http://maprantala.com/2010/12/10/zipping-a-shapefile-using-python/" target="_blank">python script </a>and an <a href="http://maprantala.com/2010/12/15/zipping-a-shapefile-via-arctoolbox/" target="_blank">ArcToolbox</a> tool for zipping a shapefile.

Well, I had a request to modify the code so it would not error out if it encounters a .lock file. While .lock files exist for a reason and shouldn&#8217;t be totally ignored, in some cases it is safe to do so, so I went ahead any modified the code, which can be downloaded from <a href="https://github.com/MapRantala/Blog/tree/master/ArcToolbox/ArcGIS_9.3/20101215_ZipShapefileFromArcToolbox" target="_blank">Github</a>.

The guts of the code is here, though:

<pre>import zipfile
import sys
import os
import glob

theShapeFile = sys.argv[1]
outputZipFile = sys.argv[2]
skipLockFile = sys.argv[3]

def zipShapefile(inShapefile, newZipFN, skipLockFile):
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
        if not ((os.path.splitext(infile.lower())[1] == ".lock") and (skipLockFile.lower() == "true")):
            zipobj.write(infile,os.path.basename(infile),zipfile.ZIP_DEFLATED)

    zipobj.close()

    return True

zipShapefile(theShapeFile,outputZipFile,skipLockFile)
print "done!"
</pre>