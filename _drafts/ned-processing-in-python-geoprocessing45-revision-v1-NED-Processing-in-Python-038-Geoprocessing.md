---
id: 4863
title: 'NED Processing in Python &#038; Geoprocessing'
date: 2019-01-20T10:04:08-06:00
author: adminguy
layout: revision
guid: https://maprantala.com/2019/01/20/45-revision-v1/
permalink: /2019/01/20/45-revision-v1/
---
The [USGS](http://www.usgs.gov/) updated a significant portion of [NED](http://ned.usgs.gov/) data for my state in June.  I recently downloaded the updates, processed them&#8211;projecting and converting the elevations from meters to feet&#8211;using python and geoprocessing.  My python skills are still pretty crude but I was able to get the job done.

One of the benefits of working for the public sector is that I can more freely publish code without worrying about &#8220;trade secrets&#8221; or what have you so I thought I would put my code out for anyone to see, maybe someone will find it useful.  It is split into two separate files, mostly because I used <a href="http://webhelp.esri.com/arcgisdesktop/9.2/index.cfm?TopicName=An_overview_of_ModelBuilder" target="_blank" rel="noopener">Model Builder</a> to generate the main processing chunk of code (Project_Reclassify.py) and another script to control looping, etc. The code isn&#8217;t pretty and you&#8217;ll see the results of my development process with two subroutines in LaunchScript.py that could really be one.

### <!--more-->

### LaunchScript.py

<pre>import Project_Reclassify

mConversion = "3.28084"
mProjection = "PROJCS['NAD_1983_UTM_Zone_15N',GEOGCS['GCS_North_American_1983',DATUM['D_North_American_1983',SPHEROID['GRS_1980',6378137.0,298.257222101]],PRIMEM['Greenwich',0.0],UNIT['Degree',0.0174532925199433]],PROJECTION['Transverse_Mercator'],PARAMETER['False_Easting',500000.0],PARAMETER['False_Northing',0.0],PARAMETER['Central_Meridian',-93.0],PARAMETER['Scale_Factor',0.9996],PARAMETER['Latitude_Of_Origin',0.0],UNIT['Meter',1.0]]"
mCellSize = "10"

#Project_Reclassify.Process("C:\mgs\Projects\NED_10m\n44w092\grdn44w092_13", mProjection, mConversion,"C:\mgs\Projects\NED_10m\n44w092\try30",mCellSize)

def CallIt (inDir,inGrid,outGrid):
fullinFN = inDir+'\'+inGrid
fulloutFN = 'C:\mgs\Projects\NED_10m\Output\'+outGrid
tmpGridFN = 'c:\temp\t'+outGrid
Project_Reclassify.Process(fullinFN, mProjection, mConversion,fulloutFN,mCellSize,tmpGridFN)

def CallIt2 (inGrd,inoutGrd):
outDir = 'C:\mgs\Projects\NED_10m\' + inGrd + '\' + inGrd

outGrd = 'grd'+inGrd+'_13'

CallIt(outDir,outGrd,inoutGrd)

#CallIt2("n44w092","LaCrosse_W")
CallIt2("n44w093","MasonCity_E")
CallIt2("n44w094","MasonCity_W")
CallIt2("n44w095","Fairmont_E")
CallIt2("n44w096","Fairmont_W")
CallIt2("n44w097","SiouxFalls_E")
#CallIt2("n45w092","EauClaire_W")
CallIt2("n45w093","StPaul_E")
CallIt2("n45w094","StPaul_W")
CallIt2("n45w095","NewUlm_E")
CallIt2("n45w096","NewUlm_W")
CallIt2("n45w097","Watertown_E")
#CallIt2("n46w093","Stillwater_E")
#CallIt2("n46w094","Stillwater_W")
#CallIt2("n46w095","StCloud_E")
CallIt2("n46w096","StCloud_W")
CallIt2("n46w097","Milbank_E")
#CallIt2("n47w092","Ashland_W")
#CallIt2("n47w093","Duluth_E")
CallIt2("n47w094","Duluth_W")
CallIt2("n47w095","Brainerd_E")
CallIt2("n47w096","Brainerd_W")
CallIt2("n47w097","Fargo_E")
#CallIt2("n48w090","Hancock_W")
#CallIt2("n48w091","TwoHarbors_E")
#CallIt2("n48w092","TwoHarbors_W")
#CallIt2("n48w093","Hibbing_E")
#CallIt2("n48w094","Hibbing_W")
CallIt2("n48w095","Bemidji_E")
CallIt2("n48w096","Bemidji_W")
CallIt2("n48w097","GrandForks_E")
#CallIt2("n48w098","GrandForks_W")
#CallIt2("n49w090","ThunderBay_W")
#CallIt2("n49w091","Quetico_E")
#CallIt2("n49w092","Quetico_W")
#CallIt2("n49w093","InternationFalls_E")
#CallIt2("n49w094","InternationFalls_W")
CallIt2("n49w095","Roseau_E")
CallIt2("n49w096","Roseau_W")
#CallIt2("n49w097","ThiefRiverFalls_E")
CallIt2("n49w098","ThiefRiverFalls_W")
#CallIt2("n50w095","Kenora_E")
#CallIt2("n50w096","Kenora_W")</pre>

### Project.Reclassify.py

<pre># Import system modules
import sys, string, os, arcgisscripting

# Create the Geoprocessor object
gp = arcgisscripting.create()

# Check out any necessary licenses
gp.CheckOutExtension("spatial")

# Load required toolboxes...
gp.AddToolbox("C:/Program Files/ArcGIS/ArcToolbox/Toolboxes/Spatial Analyst Tools.tbx")
gp.AddToolbox("C:/Program Files/ArcGIS/ArcToolbox/Toolboxes/Data Management Tools.tbx")

# Script arguments...
def Process( inInputRaster, inOutputCoord, inConversionFactor, inOutputRaster, inCellSize, intempRasterFN):
Input_Raster = inInputRaster #sys.argv[1]
Output_Coordinate_System = inOutputCoord #sys.argv[3]
Input_raster_or_constant_value_2 = inConversionFactor #sys.argv[4]
Output_CellSize = inCellSize

#if Input_raster_or_constant_value_2 == '#':
# Input_raster_or_constant_value_2 = "3.28084" # provide a default value if unspecified

Output_raster = inOutputRaster

# Local variables...
Temp_Raster = intempRasterFN

print Input_Raster
print Temp_Raster
print Output_Coordinate_System
print inCellSize

# Process: Project Raster...
gp.ProjectRaster_management(Input_Raster, Temp_Raster, Output_Coordinate_System, "CUBIC", Output_CellSize, "", "", "")

print Input_raster_or_constant_value_2
print Output_raster

# Process: Times...
gp.Times_sa(Temp_Raster, Input_raster_or_constant_value_2, Output_raster)</pre>