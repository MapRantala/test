---
id: 652
title: 'Quick &#038; Dirty python recursive folder search'
date: 2012-03-13T15:55:55-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=652
permalink: /2012/03/13/quick-dirty-python-recursive-folder-search-2/
categories:
  - arcpy
  - Python
  - 'Quick &amp; Dirty'
tags:
  - 'quick &amp; dirty python'
---
Someone asked how to have python recursively search a folder structure. There may be a better way but this is how I typically do it&#8211;it basically starts with one directory and loops through the contents compiling a list of sub-directories as it goes through the contents.

[<img class=" wp-image" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2012/03/dirstalker.png?resize=446%2C318" alt="Image" width="446" height="318" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2012/03/dirstalker.png)

<pre>import glob, os

theDir = 'c:/temp/'
theDirList = []
theDirList.append(theDir)

while len(theDirList) &gt; 0:
    newDirList = []
    for iDir in theDirList:
        print iDir
        for iFile in glob.glob(iDir+"/*"):
            if (os.path.isdir(iFile)):
                newDirList.append(iFile)

    theDirList = newDirList</pre>