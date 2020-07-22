---
id: 950
title: Garmin GPX to Shapefile (SHP) conversion GPX2Shp.py
date: 2014-05-16T21:11:20-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=950
permalink: /2014/05/16/garmin-gpx-to-shapefile-shp-conversion-gpx2shp-py/
categories:
  - Python
tags:
  - arcpy
  - geospatial
  - GIS
  - gps
  - GPX
  - python
---
I mentioned using <a href="https://tapiriik.com" target="_blank">Tapiriik</a> to batch download my entire Garmin Connect history&#8211;over 1,000 separate .GPX files. I found several tools to convert .<a href="http://arcscripts.esri.com/details.asp?dbid=16797" target="_blank">GPX to shapefiles</a> that worked but none seemed to recognize my heart rate data.

The trick is Garmin extends the GPX specification to incorporate the heart rate:

<pre><span style="color: #333333;">xmlns:gpxtpx="http://www.garmin.com/xmlschemas/TrackPointExtension/v1"</span></pre>

Each track point looks like this:

<div id="LC15" class="line">
       <trkpt lat=&#8221;43.68346489146352&#8243; lon=&#8221;-92.99583793468773&#8243;>
</div>

<div id="LC16" class="line">
          <time>2014-03-16T20:35:47+00:00</time>
</div>

<div id="LC17" class="line">
          <ele>296.20001220703125
</div>

<div id="LC18" class="line">
          <extensions>
</div>

<div id="LC19" class="line">
            <gpxtpx:TrackPointExtension>
</div>

<div id="LC20" class="line">
              <gpxtpx:hr>86
</div>

<div id="LC21" class="line">
            <!--<span class="hiddenSpellError" pre="" data-mce-bogus="1"-->gpxtpx:TrackPointExtension>
</div>

<div id="LC22" class="line">
          </extensions>
</div>

<div id="LC23" class="line">
        <!--<span class="hiddenSpellError" pre="" data-mce-bogus="1"-->trkpt>
</div>

&nbsp;

Since the first few exiting GPX converters failed to meet my needs, I decided to make my own, at least partially.

I used Joel Lawhead of <a href="http://geospatialpython.com" target="_blank">GeospatialPython.com</a>&#8216;s <a href="https://code.google.com/p/pyshp/" target="_blank">pyshp</a> library to handle writing the shapefile. I added some basic loop and I stuck a template.prj in the workspace that gets copied once for each shapefile.

Otherwise, nothing too fancy going on.  The code can be downloaded from [Github](https://github.com/MapRantala/Blog/tree/master/python/python/20140516_GPX2SHP).

<pre>import glob, os
import xml.etree.ElementTree as ET
import shapefile
import shutil

theNS = "{http://www.topografix.com/GPX/1/1}".lower()
theNS2 = "{http://www.garmin.com/xmlschemas/TrackPointExtension/v1}".lower()
templatePRJfile = "template.prj"

def elementIs(inElement,inTag):
    item1 = inTag.lower()
    item2 = elementName(inElement)
    return (inTag.lower() == elementName(inElement).lower())

def elementName(inElement):
    item1= inElement.tag.lower().replace(theNS,"").replace(theNS2,"")
    return item1

def convertTimeToSeconds(inTime):
    theSeconds = -1

    if (inTime.count(":")) == 2:
        try:
            inHour = inTime.split(":")[0]
            inMin = inTime.split(":")[1]
            inSec = inTime.split(":")[2]

            totalSec = float(inSec)
            totalSec += (float(inMin) * 60)
            totalSec += (float(inHour) * 3600)
            theSeconds = totalSec
        except:
            pass

    return theSeconds


def writeSHP(inSourceFile,inTrkList):
    w = shapefile.Writer(shapefile.POINT)
    w.field("file")
    w.field("segment","N","8",0)
    w.field("vertex","N","8",0)
    w.field("datetime","C",30)
    w.field("date","C","10",0)
    w.field("time","C","8",0)
    w.field("sec","N","8",0)
    w.field("isec","N","8",0)
    w.field("totsec","N","8",0)
    w.field("elev","N","24",14)
    w.field("hr","N","8",0)
    w.field("last","N","1",0)
    w.field("lat","N","24",16)
    w.field("lon","N","24",16)

    iTrkSegIndex = 0
    startSec =-1
    prevSec = -1
    for iTrkSeg in inTrkList:
        iTrkPtIndex = 0
        for iTrkPtDict in iTrkSeg:
            thisLine = "{0},{1},{2},*time*,*ele*,*hr*,*lat*,*lon*".format(inSourceFile,iTrkSegIndex,iTrkPtIndex)

            theLat = None
            if (iTrkPtDict.has_key('lat')):
                try:
                    theLat = float(iTrkPtDict['lat'])
                except:
                    pass

            theLon = None

            if (iTrkPtDict.has_key('lon')):
                try:
                    theLon = float(iTrkPtDict['lon'])
                except:
                    pass

            theDate = None
            theTime = None
            theSeconds = -1
            segSeconds = -1
            totSeconds = -1

            if (iTrkPtDict.has_key('time')):
                theDateTime = iTrkPtDict['time']
                if ("T" in theDateTime):
                    theDate = theDateTime.split("T")[0]
                    theTimePlue = theDateTime.split("T")[1]
                    if ("+" in theTimePlue):
                        theTime = theTimePlue.split("+")[0]
                        theSeconds = convertTimeToSeconds(theTime)

                        if (prevSec &lt; 0):
                            prevSec = theSeconds
                        if (startSec&lt;0):
                            startSec = theSeconds

                        segSeconds = theSeconds - prevSec
                        prevSec = theSeconds
                        totSeconds = theSeconds - startSec
            else:
                theDateTime = None

            if (iTrkPtDict.has_key('ele')):
                theElev = iTrkPtDict['ele']
            else:
                theElev = None

            if (iTrkPtDict.has_key('hr')):
                theHR = iTrkPtDict['hr']
            else:
                theHR = None

            if (iTrkPtIndex == len(iTrkSeg) - 1):
                theLast = 1
            else:
                theLast = 0

            w.point(theLon, theLat)
            try:
                                  w.record(inSourceFile.replace(".gpx",""),iTrkSegIndex,iTrkPtIndex,theDateTime,theDate,theTime,theSeconds,segSeconds,totSeconds,theElev,theHR,theLast,theLat,theLon)

            except:
                print "############## ERROR ####################"
            iTrkPtIndex+=1

        iTrkSegIndex+=1


    w.save(inSourceFile.lower().replace(".gpx",""))
    w = None
    if (os.path.exists(templatePRJfile)):
        newPRJFN = inSourceFile.lower().replace(".gpx",".prj")
        shutil.copyfile(templatePRJfile,newPRJFN)

def mainLoop():
    for iFile in glob.glob("*.gpx"):
        print iFile
        tree = ET.parse(iFile)
        root=tree.getroot()

        theTrkList = []

        for iRoot in root:
            if elementIs(iRoot,"trk"): #"http://www.topografix.com/gpx/1/1}trk" in iRoot.tag.lower():
                for iTrkSeg in iRoot:
                    if not elementIs(iTrkSeg,"trkseg"):
                        continue
                    thisTrk = []

                    pntIndex = 0
                    for iTrkPt in iTrkSeg:
                        if not elementIs(iTrkPt,"trkpt"):
                            continue
                        trkPntDict = dict()
                        trkPntDict["pntIndex"] = pntIndex
                        trkPntDict['lat'] = iTrkPt.get('lat')
                        trkPntDict['lon'] = iTrkPt.get('lon')

                        pntIndex+=1
                        for iElem in iTrkPt:
                            if elementIs(iElem,"extensions"):
                                for iSubElem in iElem:
                                    if (elementIs(iSubElem,"TrackPointExtension")):
                                        for iExtensionElem in iSubElem:
                                            if elementIs(iExtensionElem,"hr"):
                                                trkPntDict[elementName(iExtensionElem)] = iExtensionElem.text
                            else:
                                trkPntDict[elementName(iElem)] = iElem.text

                        #print trkPntDict
                        thisTrk.append(trkPntDict)

                    theTrkList.append(thisTrk)
        writeSHP(iFile.lower(), theTrkList)


theLineList = mainLoop()
</pre>

&nbsp;