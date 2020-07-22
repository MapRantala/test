---
id: 738
title: 'Unsupported Arc: &#8220;Rebox&#8221;ing or updating the extent of a feature class.'
date: 2012-06-05T21:16:28-05:00
author: Matt
excerpt: A useful ArcGIS 10 Add-In tool for reseting the extent of a feature class.
layout: post
guid: http://maprantala.com/?p=738
permalink: /2012/06/05/unsupported-arc-reboxing-or-updating-the-extent-of-a-feature-class/
categories:
  - ArcGIS 10
  - ArcMap
  - ArcObjects
  - ArcScripts
  - PostgreSQL
---
I&#8217;ve found that sometimes I can not find the answer to a question until I know the answer & then it becomes ridiculously easy to find the answer.

One small annoying thing that I never spent much time was when you delete features from a feature class making it significantly smaller but the envelope does not get re-sized so the zoom extent (still the original extent) is too large. This often happens to use when we convert tables to an XY theme and there are blank records&#8211;most of our data shows in Minnesota but there are some in Oklahoma (I think). Once we eliminate or correct the blank records, our data view still pops out to include a large section of the United States even though we only have data in Minnesota.

A long, long time ago, <a href="http://forums.arcgis.com/forums/54-ArcInfo-Workstation" target="_blank">Workstation ArcInfo</a> had a simple command, <a href="http://resources.esri.com/help/9.3/geoprocessing/pdf/arcinfo_commands.pdf" target="_blank">Rebox</a>, for just this purpose (actually it still does, I just don&#8217;t get to use it anymore)&#8211;it shrunk the extent to the smallest rectangle required to enclose all the data. Up until today, I thought <a href="http://forums.esri.com/Thread.asp?c=93&f=993&t=184832" target="_blank">the request</a> for this feature was completely ignored.

While researching something else, I was digging around in the sde tables and found one, sde.sde_layers, that had the interesting fields, minx, miny, maxx, and maxy. My quick & dangerous test (I performed it on a throw-away feature class in a throw-away geodatabase) gave me the results I wanted&#8211;once I loaded the feature class into ArcMap, the extent was a nice, tight rectangle around my features.

[<img class="aligncenter size-full wp-image-740" title="Extent" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2012/06/extent1.png?resize=614%2C402" alt="" width="614" height="402" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2012/06/extent1.png)

Is this a supported way to Rebox the extent? No.

Is it recommend by ESRI or me? No.

Will it screw up your entire geodatabase, making you lose all your data & costing you your job? Probably not but do you want to take that chance?

Will it get the job done? Maybe.  But in the process of writing this post, I found two safer ways to go about it. First, the straight-forward, sde command-line way that probably always existed that I never found until today, <a href="http://help.arcgis.com/en/geodatabase/10.0/admin_cmds/support_files/datamgmt/sdelayer.htm" target="_blank">sdelayer -o alter had an -E option to reset the extent</a>, including the ability to either specify it or have sde calculate it. Ok, that is usable for one person in our organization.

Previously, we had found either a VBA or other tool for doing this but had minimal success with it. Today, I found an <a href="http://resources.arcgis.com/gallery/file/Geoprocessing-Model-and-Script-Tool-Gallery/details?entryID=AE907673-1422-2418-A092-D7AAF6A16E6C" target="_blank">ArcGIS 10 Add-In</a> that is suppose to do the same thing. In my experiments (sample size n=1) it worked perfectly. If you need this sort of functionality, I would recommend trying out this Add-In first, if that fails go the sde command line route. Use the direct SQL method at your own risk!