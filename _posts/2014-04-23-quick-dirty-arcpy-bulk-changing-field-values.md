---
id: 906
title: 'Quick &#038; Dirty arcpy: Bulk Changing Field Values'
date: 2014-04-23T05:18:24-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=906
permalink: /2014/04/23/quick-dirty-arcpy-bulk-changing-field-values/
categories:
  - arcpy
  - ArcScripts
  - Python
  - 'Quick &amp; Dirty'
---
In mapping cross sections, our geologists often find themselves renaming their stratigraphic units midway, or at the end, of creating multiple cross sections.  This can cause a situation where we need to change multiple values in multiple fields in multiple feature classes&#8211;a situation that can get messy very fast.

Perfect situation for a quick & dirty arcpy script and, in this case, an [ArcToolbox tool that can be downloaded](https://github.com/MapRantala/Blog).

This tool will change all feature classes in the O:\clay\_cga\sand-distribution\_model\dnrPackages\stratlines directory.

It will look at two fields, [strat] and [unit] and make these changes:

  * &#8220;go&#8221; becomes &#8220;gro&#8221;
  * &#8220;goc&#8221; becomes &#8220;grc&#8221;
  * &#8220;sgb&#8221; becomes &#8220;grb&#8221;

And since I have Case Sensitive checked, &#8220;Go&#8221; will not get changed to &#8220;gro&#8221;.  Also note that only full values that match values in the Old Value List get changed, part matches are left as-is so &#8220;got&#8221; would be left as-is even though the first two characters match &#8220;go&#8221;.

&nbsp;

[<img class="alignnone wp-image-907 size-full" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2014/04/BulkFieldChange.png?resize=719%2C320" alt="Bulk Field Change" width="719" height="320" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2014/04/BulkFieldChange.png)

<pre>import arcpy
import sys, string, arcgisscripting
import arcpy

def printit(inString):
    print inString
    arcpy.AddMessage(inString)

def printerr(inString):
    print inString
    arcpy.AddError(inString)

def fieldExists(tablename,indexname):

 if not arcpy.Exists(tablename):
  return False

 tabledescription = arcpy.Describe(tablename)

 for iField in tabledescription.fields:
     if (iField.Name.lower() == indexname.lower()):
         return True

 return False


if len(sys.argv) &gt; 1:
    inDirectory = sys.argv[1]
    inFieldNameRaw = sys.argv[2]
    oldValue = sys.argv[3].replace(","," ")
    newValue = sys.argv[4].replace(","," ")
    caseSensitiveRaw = sys.argv[5]
else:
    inDirectory = r"C:\temp\test\stratest"
    inFieldNameRaw = "strat"
    oldValue = "go, goc, sgb".replace(","," ")
    newValue = "gro grc grb".replace(","," ")
    caseSensitiveRaw = "true"

caseSensitive = (caseSensitiveRaw.lower() == "true")
fieldNameList = inFieldNameRaw.replace(","," ").split()

printit("Starting")
printit(" Workspace: "+str(inDirectory))
printit( " inFieldName: "+str(inFieldNameRaw))
printit( " oldValue: "+str(oldValue))
printit( " newValue: "+str(newValue))
printit( " caseSensitive: "+str(caseSensitive))

valueDict = dict()

def initialQC():
    global valueDict

    if not (arcpy.Exists(inDirectory)):
        printerr("Workspace {0} does not exist".format(inDirectory))
        return False

    if (len(oldValue.split()) &lt;&gt; len(newValue.split())):
        printerr("Number of values in {0} does not equal number of values in {1}".format(oldValue,newValue))
        return False

    iValueIndex = 0
    for iOldValue in oldValue.split():
        if (caseSensitive):
            thisKey = iOldValue
        else:
            thisKey = iOldValue.lower()

        if (valueDict.has_key(thisKey)):
            printerr("ERROR: Value, {0}, is repeated, cancelling...".format(thisKey))
            return False

        valueDict[thisKey] = newValue.split()[iValueIndex]
        iValueIndex+=1
    return True

def makeFieldList(inFC):
    thisFieldList = []

    for iField in fieldNameList:
        if (fieldExists(inFC,iField)):
            thisFieldList.append(iField)

    return thisFieldList


def main():
    arcpy.env.workspace = inDirectory
    printit(valueDict)
    for iFC in arcpy.ListFeatureClasses():
        printit("Working on {0}".format(iFC))

        iFieldList = makeFieldList(iFC)
        if (len(iFieldList) == 0):
            printit(" No fields to change, Skipping...")
            continue

        rows = arcpy.UpdateCursor(iFC)

        changes = 0
        printit(" Changing Rows")
        for row in rows:
            iChange = 0
            for iField in iFieldList:
                iValue = str(row.getValue(iField))
                newValue = iValue

                if valueDict.has_key(iValue):
                    newValue = valueDict[iValue]
                else:
                    if not (caseSensitive):
                        if valueDict.has_key(iValue.lower()):
                            newValue = valueDict[iValue.lower()]

                if (newValue &lt;&gt; iValue):
                    printit("CHANGE {0}".format(newValue))
                    row.setValue(iField,newValue)
                    iChange+=1

            if (iChange &gt; 0):
                changes+=1
                rows.updateRow(row)
        printit(" Made {0} changes".format(changes))
        del row
        del rows

    printit("Main")

if (initialQC()==True):
    main()

printit("Done")
</pre>