---
id: 210
title: TopoToRaster Error
date: 2010-09-13T20:25:01-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=210
permalink: /2010/09/13/topotoraster-error/
categories:
  - Bugs
  - Python
tags:
  - ArcErrors
  - arcgis
  - ArcMap
  - Denisfy
  - ESRI
  - python
  - TopoToRaster
  - topoToraster_3d
  - TopoToRaster_sa
---
In running an automated process, I had a TopoToRaster repeatedly fail on me. The only input theme was a contour theme. The process ran fine when I used the envelope of the contour theme as the output extent but when I changed it to the envelope of a polygon theme, it would bomb. The polygon&#8217;s envelope was smaller than the contour theme.

ArcCatalog would bomb out without presenting any sort of useful message.

I determined eventually that If I set the left extent as 435210, the process would work if set the right extent to 446655 or greater but would bomb if I used 446654 or less. If the the left extent was 435209, the right extent had to be 446654 or greater.

The image below shows the contours (with vertexes) and the polygon theme I was using. The two circle graphics show the approximate extent I was clipping to.

[<img class="aligncenter size-full wp-image-211" title="topotoRaster" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2010/09/topotoraster.jpg?resize=453%2C381" alt="" width="453" height="381" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2010/09/topotoraster.jpg)

I eventually wrote python script to do a series of tests with the intent of determining the pattern of what does and does not work. The unexpected benefit was I actually got some sort of error message back. Turns out it had to do with my lack of vertices&#8211;I densified my polylines and it ran fine. Something I should have thought of earlier but it was difficult lacking any feedback.  
[<img class="aligncenter size-medium wp-image-212" title="Untitled" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2010/09/untitled.jpg?resize=535%2C271" alt="" width="535" height="271" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2010/09/untitled.jpg)