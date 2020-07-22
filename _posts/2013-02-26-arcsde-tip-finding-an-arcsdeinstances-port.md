---
id: 751
title: 'ArcSDE Tip: Finding an ArcSDE instance&#8217;s port'
date: 2013-02-26T05:25:03-06:00
author: Matt
layout: post
guid: http://maprantala.com/?p=751
permalink: /2013/02/26/arcsde-tip-finding-an-arcsdeinstances-port/
publicize_twitter_user:
  - MapRantala
categories:
  - ArcGIS
  - ArcSDE
tags:
  - ArcSDE
---
One of my main tasks right now is to document many of the details of maintaining ArcSDE geodatabases so I anticipate having several blog posts on this topic that are re-writes of documents I am working on. I am presuming that the person will have no ArcSDE experience so I am documenting very detailed information.

Almost all of the ArcSDE commands require that you specify which instance (service/port) the command applies to by using the &#8220;-i&#8221; parameter.<figure id="attachment_755" aria-describedby="caption-attachment-755" style="width: 672px" class="wp-caption aligncenter">

[<img class="size-full wp-image-755" alt="ArcSDE Instance Parameter" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2013/02/instanceparameter1.png?resize=672%2C189" width="672" height="189" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2013/02/instanceparameter1.png)<figcaption id="caption-attachment-755" class="wp-caption-text">ArcSDE Instance Parameter</figcaption></figure> 

Since we have multiple ArcSDE geodatabases, I like to have a handy-dandy sticky note with all the geodatabases and their respective ports on the side of my monitor.

But when that is not handy and I can never remember the ArcSDE command line syntax to get a list of instances and their ports&#8211;I mean remembering &#8220;<a href="http://help.arcgis.com/en/geodatabase/10.0/admin_cmds/support_files/admincmdref.htm" target="_blank">sdeservice -o list</a>&#8221; is difficult at my age.

[<img class="aligncenter size-full wp-image-756" alt="sdeservicelist" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2013/02/sdeservicelist.png?resize=586%2C264" width="586" height="264" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2013/02/sdeservicelist.png)

The quickest and most reliable way I&#8217;ve found to get the instance number is just to check the properties of your SDE connection file in ArcCatalog, right-click on it and select &#8220;Connection Properties&#8221;.

[<img class="aligncenter size-full wp-image-757" alt="connectionproperties" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2013/02/connectionproperties.png?resize=302%2C310" width="302" height="310" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2013/02/connectionproperties.png)

And the port is right there in the service entry (5164).

[<img class="aligncenter size-full wp-image-758" alt="connectionproperties2" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2013/02/connectionproperties2.png?resize=500%2C139" width="500" height="139" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2013/02/connectionproperties2.png)