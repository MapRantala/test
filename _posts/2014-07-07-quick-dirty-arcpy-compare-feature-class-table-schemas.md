---
id: 956
title: 'Quick &#038; Dirty arcpy: Compare Feature Class Table Schemas'
date: 2014-07-07T05:06:18-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=956
permalink: /2014/07/07/quick-dirty-arcpy-compare-feature-class-table-schemas/
categories:
  - ArcGIS 10
  - arcpy
  - Python
  - 'Quick &amp; Dirty'
---
I&#8217;m in the process of rewriting a process, moving most of the processing from arcpy to postgresql-enabled python (love me some <a href="http://initd.org/psycopg/" target="_blank">psycopg2</a>).

One of the QC checks I&#8217;m doing at the end of this re-write is just verifying that the feature class schemas are the same (or that the differences are intended)&nbsp; under the new process as they were in the old process.

And while ArcGIS does have a <a href="http://resources.arcgis.com/en/help/main/10.1/index.html#//001700000007000000" target="_blank">good tool</a> for this, there were a couple tweaks I wanted to make. Most notably, I wanted a list of fields that are not in both feature classes.

[<img class="alignnone size-full wp-image-957" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2014/07/TableCompare.png?resize=801%2C266" alt="ArcGIS Table Compare" width="801" height="266" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2014/07/TableCompare.png)

So I made a quick & dirty script to do that, nothing especially clever but I&#8217;ve found it useful. Download it from [GitHub](https://github.com/MapRantala/Blog/tree/master/ArcToolbox/ArcGIS_10_2/20140702_CompareTableFields). I have it currently set up to work on feature layers but you should be able to change the toolbox parameter types to allow feature classes or tables.

<pre>import arcpy,sys,os

def printit(inMessage):
    print inMessage
    arcpy.AddMessage(inMessage)

featureclass1 = sys.argv[1]
featureclass2 = sys.argv[2]

tableheaders = 'name, type, width, precision, domain'

def makeFieldDict(inFC):
    d = arcpy.Describe(inFC)
    printit("Dataset: "+d.baseName)
    printit("Type: "+d.dataType)
    printit("Path: "+d.catalogPath)
    printit(" ")

    lFields=arcpy.ListFields(inFC)

    printit (tableheaders)
    fieldDict = dict()
    printit (lFields)
    for lf in lFields:
        fieldDict[lf.name] = [lf.name,lf.type,lf.length,lf.precision,lf.domain]
        printit (lf.name+", "+lf.type +", "+str(lf.length)+", "+str(lf.precision)+", "+lf.domain)
    return fieldDict

fieldDict1 = makeFieldDict(featureclass1)
fieldDict2 = makeFieldDict(featureclass2)
errorList = []
printit(" ")
printit(" ")
printit("Comparing Fields:")
for iField in sorted(list(set(fieldDict1.keys()+fieldDict2.keys()))):
    if not (fieldDict1.has_key(iField)):
        theResult = " {0} not found in {1}".format(iField,featureclass1)
        errorList.append(theResult)
    elif not (fieldDict2.has_key(iField)):
        theResult = " {0} not found in {1}".format(iField,featureclass2)
        errorList.append(theResult)
    else:
        if (fieldDict1[iField] == fieldDict2[iField]):
            theResult = " {0} OK".format(iField)
        else:
            theResult = " {0} Have Different Definitions \n   {1}: {2}\n   {3}: {4}".format(iField,featureclass1,fieldDict1[iField],featureclass2,fieldDict2[iField])
            errorList.append(theResult)

    printit( theResult )

printit(" ")
printit(" ")
if len(errorList) == 0:
    printit("GOOD! No difference Found!")
else:
    printit("These Differences Found:")
    for iError in errorList:
        printit(iError)

printit("Done!")
</pre>