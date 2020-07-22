---
id: 776
title: 'ArcMap Field Calculator: Number of parts in multi-part feature'
date: 2013-09-05T14:04:40-05:00
author: Matt
layout: post
guid: https://nodedangles.wordpress.com/?p=776
permalink: /2013/09/05/arcmap-field-calculator/
publicize_twitter_user:
  - MapRantala
categories:
  - ArcMap
  - arcpy
  - Field Calculator
  - Python
tags:
  - ArcMap
  - arcpy
  - Field Calculator
---
In the last week, I have looked for multi-part features a couple of times. Today, I was looking for multi-part polygons after dealing with the fall-out of a case of Clip Gone Wild as shown below.

[<img style="background-image:none;padding-top:0;padding-left:0;display:inline;padding-right:0;border:0;" title="image" alt="image" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2013/09/image_thumb.png?resize=554%2C416" width="554" height="416" border="0" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2013/09/image.png)

I have not found a way to write a query to find these but Field Calculator does allow you to calculate a field’s value to the number of parts.

Using the Python parser, just write the formula (note that case matters): !shape!.partCount

[<img style="background-image:none;padding-top:0;padding-left:0;display:inline;padding-right:0;border:0;" title="image" alt="image" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2013/09/image_thumb1.png?resize=633%2C483" width="633" height="483" border="0" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2013/09/image1.png)