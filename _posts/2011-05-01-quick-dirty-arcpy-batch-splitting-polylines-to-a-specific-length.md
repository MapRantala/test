---
id: 528
title: 'Quick &#038; Dirty arcpy: Batch Splitting Polylines to a Specific Length.'
date: 2011-05-01T05:01:50-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=528
permalink: /2011/05/01/quick-dirty-arcpy-batch-splitting-polylines-to-a-specific-length/
tagazine-media:
  - 'a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:8:"14087936";s:7:"blog_id";s:8:"13690265";s:9:"mod_stamp";s:19:"2011-05-02 14:04:26";}'
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
  - arcpy
  - ESRI
  - geoprocessing
  - Programming
  - 'Quick &amp; Dirty'
tags:
  - arcgis
  - arcgis 10
  - ArcObjects
  - arcpy
  - ArcScripts
  - arctoolbox
  - ESRI
  - geoprocessing
  - polylines
  - python
---
For some odd reason, I wanted to split all the arcs in a polyline feature class to a specific length&#8211;if a specific feature was longer than the target length, it would become two or more separate polyline records.

Here is the [bare-bones script](http://dl.dropbox.com/u/22241283/NodeDangles/20110429_Polyline%20Dicer.zip) that copies an existing feature class into a new feature class then processes each record, splitting it into multiple records if the polyline is longer than the user-specified tolerance.&nbsp; Some cautionary notes:

  * This is Quick & Dirty code&#8211;minimal error catching or documentation.
  * I basically tested this against one feature class (the one I wanted to split) once I got it to work, I quit.
  * There is some rounding error&#8211;features may be a tad bit off (a few ten-thousandths of a unit).
  * I did not test against multi-part features.
  * The tolerance is the native units of the data&#8211;if your data is in meters but you want to split the polylines every mile, enter 1,609.344.

I have included both a toolbox file (.tbx) and python script (.py).&nbsp; After loading the toolbox, you&#8217;ll have to change the Source of the script by right-clicking on it, selecting the Source tab, and then navigating to the .py file.

Here is the code for the Googlebots, but you are better off [just downloading](http://dl.dropbox.com/u/22241283/NodeDangles/20110429_Polyline%20Dicer.zip) it.

<pre>import arcpy
import sys, math

def printit(inMessage):
&nbsp;&nbsp;&nbsp; print inMessage
&nbsp;&nbsp;&nbsp; arcpy.AddMessage(inMessage)

if len(sys.argv) &gt; 1:
&nbsp;&nbsp;&nbsp; inFC = sys.argv[1]
&nbsp;&nbsp;&nbsp; outFC = sys.argv[2]
&nbsp;&nbsp;&nbsp; alongDistin = sys.argv[3]
&nbsp;&nbsp;&nbsp; alongDist = float(alongDistin)
else:
&nbsp;&nbsp;&nbsp; inFC = "C:/temp/asdfasdf.mdb/jkl"
&nbsp;&nbsp;&nbsp; OutDir = "C:/temp/asdfasdf.mdb"
&nbsp;&nbsp;&nbsp; outFCName = "jkl2d"
&nbsp;&nbsp;&nbsp; outFC = OutDir+"/"+outFCName
&nbsp;&nbsp;&nbsp; alongDist = 1000

if (arcpy.Exists(inFC)):
&nbsp;&nbsp;&nbsp; print(inFC+" does exist")
else:
&nbsp;&nbsp;&nbsp; print("Cancelling, "+inFC+" does not exist")
&nbsp;&nbsp;&nbsp; sys.exit(0)

def distPoint(p1, p2):
&nbsp;&nbsp;&nbsp; calc1 = p1.X - p2.X
&nbsp;&nbsp;&nbsp; calc2 = p1.Y - p2.Y

&nbsp;&nbsp;&nbsp; return math.sqrt((calc1**2)+(calc2**2))

def midpoint(prevpoint,nextpoint,targetDist,totalDist):
&nbsp;&nbsp;&nbsp; newX = prevpoint.X + ((nextpoint.X - prevpoint.X) * (targetDist/totalDist))
&nbsp;&nbsp;&nbsp; newY = prevpoint.Y + ((nextpoint.Y - prevpoint.Y) * (targetDist/totalDist))
&nbsp;&nbsp;&nbsp; return arcpy.Point(newX, newY)

def splitShape(feat,splitDist):
&nbsp;&nbsp;&nbsp; # Count the number of points in the current multipart feature
&nbsp;&nbsp;&nbsp; #
&nbsp;&nbsp;&nbsp; partcount = feat.partCount
&nbsp;&nbsp;&nbsp; partnum = 0
&nbsp;&nbsp;&nbsp; # Enter while loop for each part in the feature (if a singlepart feature
&nbsp;&nbsp;&nbsp; # this will occur only once)
&nbsp;&nbsp;&nbsp; #
&nbsp;&nbsp;&nbsp; lineArray = arcpy.Array()

&nbsp;&nbsp;&nbsp; while partnum &lt; partcount:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; # Print the part number
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; #
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; #print "Part " + str(partnum) + ":"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; part = feat.getPart(partnum)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; #print part.count

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; totalDist = 0

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; pnt = part.next()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; pntcount = 0

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; prevpoint = None
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; shapelist = []

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; # Enter while loop for each vertex
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; #
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; while pnt:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if not (prevpoint is None):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; thisDist = distPoint(prevpoint,pnt)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; maxAdditionalDist = splitDist - totalDist

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; print thisDist, totalDist, maxAdditionalDist

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if (totalDist+thisDist)&gt; splitDist:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; while(totalDist+thisDist) &gt; splitDist:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; maxAdditionalDist = splitDist - totalDist
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; #print thisDist, totalDist, maxAdditionalDist
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; newpoint = midpoint(prevpoint,pnt,maxAdditionalDist,thisDist)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; lineArray.add(newpoint)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; shapelist.append(lineArray)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; lineArray = arcpy.Array()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; lineArray.add(newpoint)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; prevpoint = newpoint
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; thisDist = distPoint(prevpoint,pnt)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; totalDist = 0

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; lineArray.add(pnt)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; totalDist+=thisDist
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; else:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; totalDist+=thisDist
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; lineArray.add(pnt)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; #shapelist.append(lineArray)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; else:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; lineArray.add(pnt)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; totalDist = 0

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; prevpoint = pnt&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; pntcount += 1

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; pnt = part.next()

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; # If pnt is null, either the part is finished or there is an
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; #&nbsp;&nbsp; interior ring
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; #
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if not pnt:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; pnt = part.next()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if pnt:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; print "Interior Ring:"
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; partnum += 1

&nbsp;&nbsp;&nbsp; if (lineArray.count &gt; 1):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; shapelist.append(lineArray)

&nbsp;&nbsp;&nbsp; return shapelist

if arcpy.Exists(outFC):
&nbsp;&nbsp;&nbsp; arcpy.Delete_management(outFC)

arcpy.Copy_management(inFC,outFC)

#origDesc = arcpy.Describe(inFC)
#sR = origDesc.spatialReference

#revDesc = arcpy.Describe(outFC)
#revDesc.ShapeFieldName

deleterows = arcpy.UpdateCursor(outFC)
for iDRow in deleterows:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;
&nbsp;&nbsp;&nbsp;&nbsp; deleterows.deleteRow(iDRow)

del iDRow
del deleterows

inputRows = arcpy.SearchCursor(inFC)
outputRows = arcpy.InsertCursor(outFC)
fields = arcpy.ListFields(inFC)

numRecords = int(arcpy.GetCount_management(inFC).getOutput(0))
OnePercentThreshold = numRecords // 100

printit(numRecords)

iCounter = 0
iCounter2 = 0

for iInRow in inputRows:
&nbsp;&nbsp;&nbsp; inGeom = iInRow.shape
&nbsp;&nbsp;&nbsp; iCounter+=1
&nbsp;&nbsp;&nbsp; iCounter2+=1&nbsp;&nbsp; &nbsp;
&nbsp;&nbsp;&nbsp; if (iCounter2 &gt; (OnePercentThreshold+0)):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; printit("Processing Record "+str(iCounter) + " of "+ str(numRecords))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; iCounter2=0

&nbsp;&nbsp;&nbsp; if (inGeom.length &gt; alongDist):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; shapeList = splitShape(iInRow.shape,alongDist)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; for itmp in shapeList:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; newRow = outputRows.newRow()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; for ifield in fields:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if (ifield.editable):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; newRow.setValue(ifield.name,iInRow.getValue(ifield.name))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; newRow.shape = itmp
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; outputRows.insertRow(newRow)
&nbsp;&nbsp;&nbsp; else:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; outputRows.insertRow(iInRow)

del inputRows
del outputRows

printit("Done!")</pre>

<div id="geo-post-528" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>