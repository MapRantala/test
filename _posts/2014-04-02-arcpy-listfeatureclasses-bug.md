---
id: 862
title: arcpy.ListFeatureClasses() Bug
date: 2014-04-02T18:59:06-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=862
permalink: /2014/04/02/arcpy-listfeatureclasses-bug/
categories:
  - ArcObjects
  - arcpy
  - Bugs
  - Bugs
  - ESRI
tags:
  - arcgis
  - arcpy
  - ListFeatureClasses
---
I was recently re-evaluating our back-up procedures and discovered and found a nasty bug with the arcpy&#8217;s <a href="http://resources.arcgis.com/en/help/main/10.2/index.html#//018v00000018000000" target="_blank">ListFeatureClasses</a> request. If you have a feature class in a feature dataset with the same name, ListFeatureClasses may not find it or anything else in that feature dataset.

Unfortunately, we recently made our daily backup a python-based system that uses ListFeatureClasses and got bit by this bug.

After discovering missing data in our backups, I reconstructed what happened and found this bug. Below is arcpy code that iterates through the feature datasets in a geodatabase and lists the feature classes:

<pre>import arcpy

def copyAll():
    for iFeatureClass in arcpy.ListFeatureClasses():
        print(" Feature Class: {0}".format(iFeatureClass))
    iFeatureClassFull = None

testGDBname = "mgs_sandbox.sde"
arcpy.env.workspace = testGDBname

copyAll()
for iFD in arcpy.ListDatasets("","Feature"):
    print("Feature Dataset {0}:".format(iFD))
    arcpy.env.workspace = testGDBname+"/"+str(iFD)
    copyAll()</pre>

And here is a screen shot of the contents of a test enterprise geodatabase, you&#8217;ll see it has a feature data set named &#8220;outcrops&#8221; that has a feature class also named &#8220;outcrops&#8221; within it:  
[<img class="alignnone size-medium wp-image-866" alt="sandbox" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2014/04/sandbox-300x150.png?resize=300%2C150" width="300" height="150" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2014/04/sandbox.png)

And the results list only the feature dataset:

[<img class="alignnone size-medium wp-image-868" alt="results" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2014/04/results-300x184.png?resize=300%2C184" width="300" height="184" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2014/04/results.png)

But if I rename the feature dataset (e.g. outcrop_fd), the results are what I would hope for:

[<img class="alignnone size-medium wp-image-868" alt="results" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2014/04/results-300x184.png?resize=300%2C184" width="300" height="184" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2014/04/results.png) [<img class="alignnone size-medium wp-image-869" alt="results2" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2014/04/results2-300x205.png?resize=300%2C205" width="300" height="205" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2014/04/results2.png)

I found that the feature class does not even need to be within the feature dataset and also the problem does not always occur, I have had the code successfully run in some cases.

Once I confirmed the problem, I did find <a href="http://forums.arcgis.com/threads/31291-List-feature-classes-in-a-geodatabase-using-Python" target="_blank">this thread</a> from almost three years ago that mentions the bug. One poster indicated the same thing occurs in ArcObjects which leads me to think something may not be getting registered right in the sde tables.

I was not able to re-create this using either personal or file geodatabases.

So I adopting the policy of not using the same name for a feature dataset as for a feature class.

&nbsp;