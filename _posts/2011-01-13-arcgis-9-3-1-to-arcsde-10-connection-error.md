---
id: 284
title: ArcGIS 9.3.1 to ArcSDE 10 Connection Error
date: 2011-01-13T14:41:58-06:00
author: Matt
excerpt: Service Pack 2 is required to get a non-misleading error message when failing to connect an ArcGIS 9.3.1 client to an ArcSDE 10.
layout: post
guid: http://maprantala.com/?p=284
permalink: /2011/01/13/arcgis-9-3-1-to-arcsde-10-connection-error/
geo_latitude:
  - "44.979965"
geo_longitude:
  - "-93.263836"
geo_accuracy:
  - "42000"
geo_address:
  - S Washington Ave, Minneapolis, MN 55415
geo_public:
  - "1"
categories:
  - ArcGIS
  - ArcGIS 10
  - ArcGIS 9.3.1
  - ArcSDE
  - Bugs
  - ESRI
  - Misc
tags:
  - arcgis 10
  - ArcGIS 9.3.1
  - ArcSDE
  - Bugs
  - ESRI
  - node dangles
  - postgis
  - postgres
---
![](/Users/mrantala/AppData/Local/Temp/moz-screenshot-7.png)We finally installed an instance of ArcSDE 10 today.  My first attempt at connecting in ArcCatalog 9.3.1 failed with the following error:

> Failed to connect to the specified server.
> 
> This release of the GeoDatabase is either invalid or out of date. [Please run the
> 
> ArcSDE setup utility using the -o install option.]
> 
> DBMS table not found[sde.sde.GDB_Release]

[<img class="alignnone size-full wp-image-288" title="ConnectError2" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2011/01/connecterror2.jpg?resize=630%2C315" alt="" width="630" height="315" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2011/01/connecterror2.jpg)

&nbsp;

Turns out the solution was simple, this [article](http://resources.arcgis.com/content/kbase?fa=articleShow&d=37385) points out that [Service pack 2](http://resources.arcgis.com/content/patches-and-service-packs?fa=viewPatch&PID=17&MetaID=1620) is required.  After updating my ArcGIS, the problem was solved:

[<img class="aligncenter size-full wp-image-295" title="ErrorMessage" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/01/errormessage.jpg?resize=397%2C187" alt="" width="397" height="187" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/01/errormessage.jpg)

I was the proud, new recipient of a non-misleading error message.  After some minor head-slapping, I realized that [9.3.1 and older versions of ArcGIS](http://wikis.esri.com/wiki/display/SupportCenter/ArcGIS+10+-+Product+Compatibility+Matrix+for+Clients+and+Servers+of+ArcSDE+technology+in+ArcGIS+Server) are not able to connect to ArcSDE 10.  Doh!  My bad&#8211;I should have read ESRI&#8217;s [clear information](http://help.arcgis.com/en/arcgisdesktop/10.0/help/index.html#/Compatibility_between_clients_and_geodatabases_in_PostgreSQL_when_using_an_ArcSDE_service/002p000000pw000000/) on this topic.  At least I never actually tried to install ArcSDe with the -o option as the first error message instructed me to do.

<div id="geo-post-284" class="geo geo-post" style="display: none">
  <span class="latitude">44.979965</span><span class="longitude">-93.263836</span>
</div>