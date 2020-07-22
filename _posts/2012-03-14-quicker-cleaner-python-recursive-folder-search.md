---
id: 657
title: 'Quicker &#038; Cleaner python recursive folder search'
date: 2012-03-14T17:28:59-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=657
permalink: /2012/03/14/quicker-cleaner-python-recursive-folder-search/
categories:
  - arcpy
  - ESRI
  - Python
  - 'Quick &amp; Dirty'
---
As contributor of the day, [Jason Scheirer](http://www.cleanstick.net/jason/), [pointed out](http://maprantala.com/2012/03/13/quick-dirty-python-recursive-folder-search-2/#comments), python has a simple, direct way to browse through the subdirectories of a directory&#8211;[os.walk](http://docs.python.org/library/os.html#os.walk)

Here is a bare-bones example of using it to print out the subdirectories in a path. The files variable of the 3-tuple is a list of files similar to the dirs variable that I loop through.

Thanks Jason for pointing out something I missed.

<pre>import os

theDir = 'c:/temp/'

for root, dirs, files in os.walk(theDir,True,None):
    for idir in dirs:
        print "     directory:   {0}/{1}".format(root,idir)</pre>

[<img class="aligncenter size-full wp-image-658" title="DirStalker2" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2012/03/dirstalker21.png?resize=590%2C239" alt="" width="590" height="239" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2012/03/dirstalker21.png)