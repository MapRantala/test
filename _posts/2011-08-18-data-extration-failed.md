---
id: 628
title: 'Data Extration Failed &#8212; Reverse Proxy Time-out'
date: 2011-08-18T19:55:47-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=628
permalink: /2011/08/18/data-extration-failed/
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
  - ArcGIS
  - ArcGIS 10
  - ArcGIS Server
  - ArcSDE
  - ESRI
tags:
  - arc gis server
  - arcforum
  - data extraction
  - distributed geodatabases
---
Testing one of our geodata services, we discovered that it allowed us to extract a portion of our feature class but when we tried to extract the entire data set, we received this Data Extraction error: Data extraction failed. Proxy or Gateway Server did not allow the URL. Check with your LAN administrator that Proxy or Gateway server is configured to allow the URL.

The fact that I was able to extract a portion of the data and I could see the entire geodatabase get made and zipped led me to believe it was more of time-out issue.

<p style="text-align:center;">
  <img class="size-full wp-image-629 aligncenter" title="Data Extraction Failed" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2011/08/dataextractionfailed.png?resize=475%2C205" alt="" width="475" height="205" data-recalc-dims="1" />
</p>

Reading through this thread at [ArcForum](http://forums.esri.com/thread.asp?c=158&f=1696&t=273750) led to some good information.  But Thomas&#8217; comment that he was using &#8220;IIS7 for my reverse proxy server&#8221; and had to change one more setting led me to the solution.  In Server Manager, the default Proxy Time-Out is set at 30 seconds by default.  I bumped that up, 60 seconds shown below but I ended up going to 300 seconds and the problem was resolved.

<p style="text-align:center;">
  <a href="https://i0.wp.com/maprantala.com/wp-content/uploads/2011/08/servermanager.jpg"><img class="size-full wp-image-630 aligncenter" title="Server Manager" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2011/08/servermanager.jpg?resize=614%2C339" alt="" width="614" height="339" data-recalc-dims="1" /></a>
</p>

<div id="geo-post-628" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>