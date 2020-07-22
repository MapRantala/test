---
id: 783
title: 'Arcpy: Check if a field exists'
date: 2013-09-23T06:47:36-05:00
author: Matt
excerpt: Check to see if a field exists in arcpy
layout: post
guid: https://nodedangles.wordpress.com/?p=783
permalink: /2013/09/23/arcpy-check-if-a-field-exists/
publicize_twitter_user:
  - MapRantala
tagazine-media:
  - 'a:7:{s:7:"primary";s:0:"";s:6:"images";a:2:{s:57:"http://nodedangles.files.wordpress.com/2013/09/image2.png";a:6:{s:8:"file_url";s:57:"http://nodedangles.files.wordpress.com/2013/09/image2.png";s:5:"width";i:385;s:6:"height";i:111;s:4:"type";s:5:"image";s:4:"area";i:42735;s:9:"file_path";b:0;}s:63:"http://nodedangles.files.wordpress.com/2013/09/image_thumb2.png";a:6:{s:8:"file_url";s:63:"http://nodedangles.files.wordpress.com/2013/09/image_thumb2.png";s:5:"width";i:445;s:6:"height";i:131;s:4:"type";s:5:"image";s:4:"area";i:58295;s:9:"file_path";b:0;}}s:6:"videos";a:0:{}s:11:"image_count";i:2;s:6:"author";s:8:"14087936";s:7:"blog_id";s:8:"13690265";s:9:"mod_stamp";s:19:"2013-09-20 13:07:02";}'
categories:
  - arcpy
  - ArcScripts
  - Programming
  - Python
tags:
  - arcpy
  - field exists
---
I was helping a co-worker who needed to check if a field exists in their arcpy script. Since we were located at their computer, I thought I would just do a quick Google search and pull the code off this blog. Seemed logical since I the original purpose was exactly that—to serve as a handy, public place to store code snippets that I use & that others might find handy.

Anyhow, my Google Search on “[Node Dangles field Exists](https://www.google.com/#q=Node+Dangles+field+Exists)” came up with a [9.3 script](http://maprantala.com/2011/01/27/checking-to-see-if-a-field-index-exists-using-python/) to check if field index exists. And I also have a [10.0 version](http://maprantala.com/2011/02/21/checking-to-see-if-a-field-index-exists-using-arcpy-argis-10-0/) but did not come up with the field exists snippet. So here it is:

[<img style="background-image: none; padding-top: 0; padding-left: 0; display: inline; padding-right: 0; border: 0;" title="image" alt="image" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2013/09/image_thumb2.png?resize=445%2C131" width="445" height="131" border="0" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2013/09/image2.png)

<pre>def fieldExists(inFeatureClass, inFieldName):
   fieldList = arcpy.ListFields(inFeatureClass)
   for iField in fieldList:
      if iField.name.lower() == inFieldName.lower():
         return True
   return False
</pre>