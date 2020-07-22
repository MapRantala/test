---
id: 304
title: Using ArcGIS 9.3.1 Desktop to Extract Data From an ArcGIS 10 Server Geodata Service
date: 2011-02-04T18:26:53-06:00
author: Matt
excerpt: Extract data from an ArcGIS 10 Server dataset into a 9.3.1 geodatabase using XML.
layout: post
guid: http://maprantala.com/?p=304
permalink: /2011/02/04/using-arcgis-9-3-1-desktop-to-extract-data-from-an-arcgis-10-server-geodata-service/
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
categories:
  - ArcGIS 10
  - ArcGIS 9.3.1
  - ArcGIS Server
  - ArcSDE
  - Misc
tags:
  - arcgis
  - ArcGIS 9.3.1
  - ArcGIS Server
  - ArcSDE
  - Distributed Geodatabase Toolbar
  - Extract to XML
  - Geodatabase version 10
  - Geodatabase version 9.3
  - node dangles
  - XML
---
In building our Enterprise GIS Database, we need to support users with different needs.  Some of our users just need to see the data on a map while others may want to download a copy of the data so they can use it within their own desktop system.

After doing some exploring, one of the options that looks like it will feel the bulk of our internal needs is to create a [Map Service](http://webhelp.esri.com/arcgisdesktop/9.3/index.cfm?TopicName=Types_of_services_in_ArcGIS_Server&anchor=ArcGIS%20Map%20service)/[Geodata Servic](http://webhelp.esri.com/arcgisdesktop/9.3/index.cfm?TopicName=Types_of_services_in_ArcGIS_Server&anchor=ArcGIS%20Geodata%20service)e pair&#8211;by creating a Map Service, we can make an easy-to-use visual representation of our data.  Combining that with a paired Geodata Service&#8211;a Geodata Service with the same name as the Map Service&#8211;we can accommodate those users who want to extract the data to their hard drive.

A user who has added paired Map & Geodata Services to an ArcMap dataframe can use the Distributed Geodatabase Toolbar to extract data to a local geodatabase.  The Extract Data button is on the far right of the Distributed Geodatabase Toolbar.

[<img class="alignnone size-full wp-image-305" title="Distributed Geodatabase Toolbar" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2011/02/distributedgeodatabasetoolbar.jpg?resize=463%2C333" alt="" width="463" height="333" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2011/02/distributedgeodatabasetoolbar.jpg)

&nbsp;

Unfortunately, my first attempts at testing this did not work.  It would look like it was processing, I would end up with a .mdb or .gdb file on my hard drive but I could not open it.  I would get a &#8220;Failed to connect to database. This release of the GeoDatabase is eitehr invalid or out of date. [Unknown GeoDatabase release.]  The Microsoft Jet database engine cannot find the input table or query &#8216;GDB_ReleaseInfo&#8217;. Make sure it exists and that its name is spelled correctly.&#8221; error.

[<img class="alignnone size-full wp-image-308" title="Version Error" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2011/02/versionerror.jpg?resize=478%2C236" alt="" width="478" height="236" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2011/02/versionerror.jpg)

&nbsp;

When exporting to an .mdb file, I could open it in Access and see the data.

Luckily the error is fairly helpful&#8211;I was getting an ArcGIS [10 Geodatabase](http://blogs.esri.com/Dev/blogs/geodatabase/archive/2010/03/15/The-Simplified-Geodatabase-Schema-in-ArcGIS-10.aspx) even though I am running ArcGIS 9.3.1 on my desktop.  I was able to open the exported geodatabases using a copy of ArcGIS 10 on another machine.   The reason for the inconsistency is pretty interesting.  Our office is, and for the foreseeable future, running in a mixed-version environment.  We have a mixture of 9.3.1 and 10 desktops, a 9.3.1 ArcSDE server and a 10 ArcGIS Server.  The Extract Tool is a server-side function apparently that creates a version 10 Geodatabase.

The way to get around this is to extract the data to XML and then import the XML workspace into a geodatabase.  Not sure if that will work for all schemas but it was successful for me.

[<img class="alignnone size-full wp-image-309" title="Export XML" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2011/02/exportxml.jpg?resize=613%2C512" alt="" width="613" height="512" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2011/02/exportxml.jpg)

&nbsp;

<div id="geo-post-304" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>