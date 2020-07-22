---
id: 215
title: Another TopoToRaster Error
date: 2010-09-15T13:36:34-05:00
author: Matt
excerpt: TopoToRaster requires some variation in your surface.
layout: post
guid: http://maprantala.com/?p=215
permalink: /2010/09/15/another-topotoraster-error/
categories:
  - ArcGIS
  - Bugs
  - Misc
tags:
  - ArcMap
  - error messages
  - ESRI
  - GIS
  - spatial analyst
  - TopoToRaster
  - topoToraster_3d
  - TopoToRaster_sa
---
### Subtitled:  Why error messages are good.

Came up with another error while running TopoToRaster but this time ArcGIS gave an error message that led to a solution.  Turned out all my contour lines had an elevation of 16 which TopoToRaster did not like.  I had intended to increase the elevation and inadvertently set them all to sixteen.  I had saved the previous values before editing so it turned out to be a simple fix and I didn&#8217;t have to spend a day trying figure out what was wrong.  
[<img class="aligncenter size-full wp-image-219" title="Sixteen" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2010/09/sixteen.jpg?resize=630%2C225" alt="" width="630" height="225" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2010/09/sixteen.jpg)

[  
](http://maprantala.com/wp-content/uploads/2010/09/topotorastererror1.jpg)