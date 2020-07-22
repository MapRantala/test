---
id: 36
title: Sticky Move Tolerance
date: 2010-07-01T16:51:42-05:00
author: Matt
excerpt: The Sticky Move Tolerance is an important setting when editing data in ArcMap.
layout: post
guid: http://maprantala.com/?p=36
permalink: /2010/07/01/sitcky-move-tolerance/
categories:
  - ArcMap
tags:
  - ArcMap
  - Sticky Move Tolerance
---
While I am pretty experienced in editing data in previous generations of ESRI&#8217;s software, I have not done a lot in ArcGIS. So in my new position where I am supporting a group of geologists who do a lot of data creation in ArcMap, I am learning some of the intricacies of the ArcGIS platform.

One of the potential gotchas that a coworker informed me about that I was vaguely aware of was the issue of sticky move tolerances. What can happen, when editing existing features, is that each time you select a feature it can easily get moved very slightly if the mouse moves while the feature is selected. While the movement itself may be well within any allowable accuracy requirements, it can make a huge mess if you are working with a shapefile polygon coverage for example.

ArcMap has a setting, Sticky Move Tolerance, that helps prevent these tiny, accidental moving of features.  A feature will not be moved until the mouse moves at least as many screen pixels as this setting.  The default setting is 0, disabling this feature.

This setting can be set changed under Options on the Editor Toolbar.  
[<img class="alignnone size-medium wp-image-37" title="ArcMap Editor Setting" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2010/07/setting.jpg?resize=160%2C393" alt="" width="160" height="393" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2010/07/setting.jpg)

The Sticky move tolerance is on the General tab.  In this example, I have my setting at 25 pixels which may be a bit high but I do not do a lot of data editing myself so it is workable.  The downside of having it set this high is if I want to move a feature just a tiny bit, I have to first drag the mouse relatively far away and then bring it tighter into where the feature is.

[<img class="alignnone size-medium wp-image-38" title="Options" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2010/07/options.jpg?resize=344%2C341" alt="" width="344" height="341" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2010/07/options.jpg)

Not sure how well this is documented although it can be found in the help system.

<a href="http://webhelp.esri.com/arcgisdesktop/9.3/index.cfm?TopicName=Moving_features" target="_blank" rel="noopener">http://webhelp.esri.com/arcgisdesktop/9.3/index.cfm?TopicName=Moving_features</a>