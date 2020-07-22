---
id: 4514
title: Calling os.startfile and webbrowser.open from ArcGIS.
date: 2014-12-31T14:38:16-06:00
author: Matt
layout: post
guid: http://maprantala.com/?p=4514
permalink: /2014/12/31/calling-os-startfile-and-webbrowser-open-from-arcgis/
categories:
  - arcpy
  - ArcScripts
  - Bugs
  - Python
tags:
  - arcpy
  - ESRI
  - python
---
Recently I&#8217;ve created <a href="http://resources.arcgis.com/en/help/main/10.2/index.html#//014p00000025000000" target="_blank">Python add-ins</a> for data entry for our staff. Most of these have a toolbar with a &#8220;Help&#8221; button that opens a help file in .pdf format.<figure id="attachment_4515" aria-describedby="caption-attachment-4515" style="width: 339px" class="wp-caption alignnone">

[<img class="size-full wp-image-4515" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2014/12/Toolbar.png?resize=339%2C66" alt="Sample python add-in toolbar." width="339" height="66" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2014/12/Toolbar.png)<figcaption id="caption-attachment-4515" class="wp-caption-text">Sample python add-in toolbar.</figcaption></figure> 

The first add-in was for ArcCatalog and this worked splendidly. I was using os.startfile(_path to help.pdf_).

However, when I started doing ArcMap add-ins, clicking the Help button would open the help.pdf but ArcMap would crash. Oops!

Luckily the <a href="https://twitter.com/arcpy" target="_blank">Python development team at Esri</a> already had a <a href="https://arcpy.wordpress.com/2013/10/25/using-os-startfile-and-webbrowser-open-in-arcgis-for-desktop/" target="_blank">blog post</a> about this at their <a href="https://arcpy.wordpress.com/" target="_blank">ArcPy Café blog</a>.

They report that the root of the problem is &#8220;conflicts in the way the Windows libraries expect to be called, they can fail or crash when called within ArcGIS for Desktop in an add-in script or geoprocessing script tool&#8221;. But this can be overcome by using a <a href="http://en.wikipedia.org/wiki/Decorator_pattern" target="_blank">decorator</a> function that calls os.startfile from a new thread. Another function effected by these conflicts is webbrowser.open.

Example code is shown below:

<pre>import functools
import os
import threading
import webbrowser
 
# A decorator that will run its wrapped function in a new thread
def run_in_other_thread(function):
    # functool.wraps will copy over the docstring and some other metadata
    # from the original function
    @functools.wraps(function)
    def fn_(*args, **kwargs):
        thread = threading.Thread(target=function, args=args, kwargs=kwargs)
        thread.start()
        thread.join()
    return fn_
 
# Our new wrapped versions of os.startfile and webbrowser.open
startfile = run_in_other_thread(os.startfile)
openbrowser = run_in_other_thread(webbrowser.open)</pre>

Then whenever you call startfile or openbrowser, it will be routed through your decorator function and, as far as I&#8217;ve been able to tell, works fine without crashing your ArcMap session.

Cheers!