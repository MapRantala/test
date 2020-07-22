---
id: 4521
title: Converting MXD to Layer file in Arcpy
date: 2015-08-14T10:01:54-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=4521
permalink: /2015/08/14/converting-mxd-to-layer-file-in-arcpy/
categories:
  - ArcGIS
  - arcpy
  - ArcScripts
  - Python
tags:
  - arcgis
  - arcpy
  - ArcScripts
  - ESRI
  - GIS
  - lyr
  - mxd
  - python
---
Working on doing some <a href="http://resources.arcgis.com/EN/HELP/MAIN/10.1/index.html#//01540000056t000000" target="_blank">advanced ArcGIS server printing</a>&nbsp;and had the need to batch convert many existing .mxd files to .lyr files. So instead of opening up X number of map documents, thought I would do it via code. All of my .mxds in this case had just one data frame so the process was pretty simple&#8211;I add an empty group layer (Thanks <a href="http://gis.stackexchange.com/users/434/petr-krebs" target="_blank">Petr Krebs</a> for the idea), copy all the existing layers into it, and save it out as a layer file.

I created an ArcGIS toolbox with two options&#8211;one to convert a single .mxd and one to batch convert an entire folder. To use it, make sure to have the EmptyGroup.lyr in the same directory as the .py file.

Here is the raw code or <a href="https://github.com/MapRantala/Blog/tree/master/ArcToolbox/ArcGIS/20150814_ConvertMxd2Lyr" target="_blank">git it</a>:

<pre>import os
import arcpy
import inspect
import glob
import uuid
import inspect

codeDir = os.path.dirname(inspect.getfile(inspect.currentframe()))
EmptyGroupLayerFile = codeDir+"/EmptyGroup.lyr"
inArg1 = sys.argv[1]
inArg2 = sys.argv[2]

def printit(inMessage):
    arcpy.AddMessage(inMessage)

def makeLyrFromMXD(inMXD, outLyr):
    if not (os.path.exists(inMXD)):
        printit( "ERROR: {} does not exist".format(inMXD))
        return False
    if not (os.path.exists(EmptyGroupLayerFile)):
        printit( "ERROR: {} does not exist".format(EmptyGroupLayerFile))
        return False
    if  (os.path.exists(outLyr)):
        printit( "Skipping: {} already exists".format(outLyr))
        return True

    printit( "Making Layer file: {0}".format(outLyr))

    mxd = arcpy.mapping.MapDocument(inMXD)
    ###Right now, just doing the first Dataframe, this could be modified
    df = arcpy.mapping.ListDataFrames(mxd)[0]

    theUUID = str(uuid.uuid1())

    iGroupLayerRaw = arcpy.mapping.Layer(EmptyGroupLayerFile)
    iGroupLayerRaw.name = theUUID
    arcpy.mapping.AddLayer(df,iGroupLayerRaw,"TOP")
    groupBaseName = os.path.basename(outLyr).split(".")[0]

    for lyr in arcpy.mapping.ListLayers(df):
        if not (lyr.name == theUUID):
            if (lyr.longName == lyr.name):
                arcpy.mapping.AddLayerToGroup (df, iGroupLayer, lyr, "Bottom")
        else:
            iGroupLayer = lyr

    iGroupLayer.name = groupBaseName
    arcpy.SaveToLayerFile_management(iGroupLayer, outLyr)
    return os.path.exists(outLyr)

def doMultiple(inDir,outDir):
    for iMxd in glob.glob(inDir+"/*.mxd"):
        lyrFile = outDir+"/"+os.path.basename(iMxd).lower().replace(".mxd",".lyr")
        makeLyrFromMXD(iMxd, lyrFile)

if(not os.path.exists(EmptyGroupLayerFile)):
    printit("Error: {} is missing, can not run.".format(EmptyGroupLayerFile))
else:
    if (os.path.isdir(inArg1) and (os.path.isdir(inArg2))):
        doMultiple(inArg1,inArg2)
    elif (os.path.isfile(inArg1)):
        if (os.path.exists(inArg2)):
            printit("Error: {} already exists".format(inArg2))
        else:
            makeLyrFromMXD(inArg1,inArg2)
    else:
        printit("Unable to understand input parameters")
</pre>