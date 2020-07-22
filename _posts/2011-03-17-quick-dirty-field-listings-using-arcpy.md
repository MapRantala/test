---
id: 473
title: 'Quick &#038; Dirty arcpy: Field Listings'
date: 2011-03-17T17:03:39-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=473
permalink: /2011/03/17/quick-dirty-field-listings-using-arcpy/
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
  - Python
  - 'Quick &amp; Dirty'
tags:
  - arcgis 10
  - arcobject
  - arcpy
  - domain
  - ESRI
  - geoprocessing
  - record counts
  - table fields
---
I have to often get a table structure for a feature class or table into either a spreadsheet or word processing document.  There might be an easy way to do this in ArcGIS 10 but I haven&#8217;t found it.  So, as is my nature, I decided to roll my own.

This is a [bare-bones script](http://dl.dropbox.com/u/22241283/NodeDangles/20110316_ListFieldCounts-arcpy.zip) that iterates through the fields, printing the field name, type, width, and precision.  There are three optional features to it:

  * You can choose to have it list the domain, if there is one, on each field.
  * You can have it write to a text file (otherwise you can just copy & paste the results from the results window).
  * You can have it count the number of populated records.  This can take a long time if working with a large dataset.  Also note that my logic for determining what constitutes being populated may not be what you need but the structure is there.  I also do not account for all field types, if the field is of a type I have not account for, the code will return -999.

To use the script from ArcToolbox, you need to pass it four parameters, their Names, type, whether they are input or output, and whether they are required or optional are:

  * featureclass, Table, Input, Required
  * includedomainstring, Boolean, Input, Required (controls whether or not domains are exported)
  * doCountsRespone, Boolean, Input, Required (controls whether or not you want to get the number of populated records.  (Your definition of populated may vary from my code)
  * outputFile, File, Output, Optional (optional output file to write)

Here is the code, but you are better off [just downloading](http://dl.dropbox.com/u/22241283/NodeDangles/20110316_ListFieldCounts-arcpy.zip) it since I haven&#8217;t figured out a good way to have WordPress play nicely with python&#8217;s indenting.

<pre># Name: ListFields-arcpy.py
#
# Purpose: Lists the fields, their type, width, and precision
# Can either have it export it to a CSV file or copy
# and paste from the results window.
#
# To use, create a tool from the script and add 3 parameters:
#  1) Table, Input, Required
#  2) Boolean, Input, Required (controls whether or not domains are exported)
#  3) Boolean, Input, Rekquired (controls whether or not you want to get the number of
#  Populated records.&nbsp; (Your defintion of populated may vary from my code)
#  4) File, Output, Optional (optional output file to write)
#
#

import arcpy,sys,os

def printit(inMessage):
 print inMessage
 arcpy.AddMessage(inMessage)

if len(sys.argv) > 4:
 featureclass = sys.argv[1]
 includedomainstring = sys.argv[2]
 doCountsRespone = sys.argv[3]
 outputFile = sys.argv[4]
else:
 featureclass = "C:/temp/before.shp"
 includedomainstring = "false"
 doCountsRespone = "true"
 outputFile = "C:/temp/before.csv"

if (outputFile == ""):
 doOutputFile = False
else:
 doOutputFile = True

if (str(doCountsRespone).lower() == "true"):
 doCounts = True
else:
 doCounts = False

if (str(includedomainstring).lower() == "true"):
 includedomain = True
else:
 includedomain = False

lfields=arcpy.ListFields(featureclass)

d = arcpy.Describe(featureclass)
printit("Dataset: "+d.baseName)
printit("Type: "+d.dataType)
printit("Path: "+d.catalogPath)
printit(" ")

tableheaders = 'name,type,width,precision'

if (doCounts == True):
 tableheaders+=",count"

if (includedomain == True):
 tableheaders+=",domain"

if (doOutputFile):
 tmpfile = open(outputFile,"w")
 tmpfile.write(tableheaders)
 tmpfile.write("n")

printit (tableheaders)
for lf in lfields:

 pThisline = lf.name+","+lf.type +","+str(lf.length)+","+str(lf.precision)

 if (doCounts == True):

 rowCount = 0

 #Note that I do not account for all field types
 #Also note that my definition of being populated may vary from yours.
 #I am using -999 as a flag to indicate a field type was not successfully
 #identified.
 if (lf.type == "Double") or (lf.type == "Single")&nbsp; or (lf.type == "Integer") or (lf.type == "SmallInteger"):
  queryString = '"'+lf.name + '" &gt; 0'
  rows = arcpy.SearchCursor(featureclass, queryString, "", "", "")
 elif (lf.type == "String"):
  queryString = '"'+lf.name + '" &lt;&gt; ' + "''"
  rows = arcpy.SearchCursor(featureclass, queryString, "", "", "")
 else:
  rowCount = -999
  #rows = arcpy.SearchCursor(featureclass, "", "", "", "")

 if (rowCount == 0):
  for row in rows:
   rowCount+=1

 pThisline=pThisline+","+str(rowCount)

 if (includedomain == True):
  pThisline=pThisline+","+lf.domain

 printit (pThisline)

 if (doOutputFile):
  tmpfile.write(pThisline)
  tmpfile.write("n")

if (doOutputFile):
 tmpfile.close</pre>

<div id="geo-post-473" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>