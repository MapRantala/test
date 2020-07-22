---
id: 620
title: 'ArcMap Field Calculator: Create a Unique ID'
date: 2011-07-28T20:08:13-05:00
author: Matt
excerpt: How to calculate a sequential unique ID in ArcGIS 10 using an arcpy module in the ArcMap Field Calculator.
layout: post
guid: http://maprantala.com/?p=620
permalink: /2011/07/28/using-arcpy-in-arcgis-10-field-calculater-to-create-a-unique-id/
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
twitter_cards_summary_img_size:
  - 'a:6:{i:0;i:671;i:1;i:574;i:2;i:3;i:3;s:24:"width="671" height="574"";s:4:"bits";i:8;s:4:"mime";s:9:"image/png";}'
categories:
  - ArcGIS
  - ArcGIS 10
  - ArcMap
  - arcpy
  - ArcScripts
  - Field Calculator
  - Python
tags:
  - arcgis 10
  - arcpy
  - ArcView 3.x
  - Field Calculator
---
One of the common functions I have to do is assign each record in a feature class with a unique identifier&#8211;normally just a sequential number from 1 to N.  In ArcView 3.x, the formula was simply &#8220;rec + 1&#8221; if I wanted to start with the number 1.

In ArcGIS, the process got a little more complex&#8211;you had to write a little VBA in Field Calculator as [described by ESRI](http://support.esri.com/en/knowledgebase/techarticles/detail/27427).

While this option still exists in ArcGIS 10, I believe it will disappear when 10.1 comes out and VBA support is completely eliminated.  But it is doable using Python which will continue to be supported.

Googling around, I did not find an exact answer but Dave Verbyla, Professor of GIS/Remote Sensing at the University of Alaska has a [posted some samples](http://nrm.salrm.uaf.edu/~dverbyla/nrm638/lectures/Python_field_calculator.pdf) that served as a good starting point.

In the Pre-Logic Script Code box, I declare a variable (counter) and a function. Then in the formula, I call the function.

<pre>counter = 0
def uniqueID():
  global counter
  counter += 1
  return counter</pre>

[<img class="alignleft size-full wp-image-621" title="Field Calculator" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/07/fc-arcpy.png?resize=614%2C525" alt="" width="614" height="525" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/07/fc-arcpy.png)

While composing this post, I actually wanted a concatenated value; &#8220;OC&#8221; plus an 8 character numeric sequential number starting at OC00000001 so the actual code is shown below:

[<img class="alignright size-full wp-image-623" title="Field Calculator" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/07/fc-arcpy2.png?resize=480%2C578" alt="" width="480" height="578" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/07/fc-arcpy2.png)

<div id="geo-post-620" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>