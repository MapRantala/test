---
id: 712
title: Building a local, permanent cache of Bing Imagery.
date: 2012-04-30T05:28:15-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=712
permalink: /2012/04/30/building-a-local-permanent-cache-of-bing-imagery/
tagazine-media:
  - 'a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:8:"14087936";s:7:"blog_id";s:8:"13690265";s:9:"mod_stamp";s:19:"2012-04-27 14:19:04";}'
categories:
  - ArcGIS 10
  - arcpy
  - Programming
tags:
  - ArcBrutile
  - ArcForums
  - arcgis 10
  - bing imagery
  - bing maps
  - Mosaic Dataset
---
I recently had an internal request to capture and store the Bing imagery for an area for future use. The user was interested in some specific images that were taken after a fire, making the ground surface-and certain geological features-much more visible. His concern was that in the future this imagery might get replaced with updated imagery taken when the vegetation has grown back.

Since it is unknown when/how this data might be used by us, we mostly wanted to capture it now & find a way to use it.

While we initiated the process of finding out what agency the data was available through, we also came up with a quick & dirty way to download the data.

Since ArcGIS 10 has made the process of loading cached basemap data a trivial process through ArcGIS Online, I have not used it much since taking a [quick first look](http://maprantala.com/2010/09/09/arcbrutile/) at it in 2010.

After removing my old, forgotten version and installing the latest, shiniest version of ArcBruTile, I verified it was able to display the imagery we wanted. ArcBruTile can be used to &#8220;display maps from [OpenStreetMap](http://www.openstreetmap.org/), [Bing](http://www.bing.com/maps/),  [SpatialCloud](http://www.spatialcloud.com/), [MapQues](http://www.mapquest.com/)t,  [Europa Technologies](http://www.europa-tech.com/), [VR-TheWorld Online](http://www.vr-theworld.com/), [Mapbox](http://www.mapbox.com/), [Stamen Design](http://www.stamen.com/) and others in ArcGIS Desktop&#8221;. The cool thing for me was it builds a local cache in an open format&#8211;a bunch of jpeg files in a directory structure. All I had to do was clear the cache, and pan through the area of interest at the desired scale.

I could either spend many long boring hours manually panning, go through the process of renting a chimp to do it for me, or write some code to do it for me. I ended up making a fishnet of the area of interest and wrote a python script to pan through the area (to be posted).

After I had the images, I ended up build a Mosaic Dataset and added the images to it.  The last trick I that I had to figure out&#8211;and really I just found in it [ArcForums](http://forums.arcgis.com/)&#8211;was how to [create a mosaic dataset using relative paths](http://forums.arcgis.com/threads/50978-Relative-paths-in-mosaic-datasets?p=193579#post193579). Can not be done, at least in 10, but by using the &#8220;Repair&#8230;&#8221; option to reset paths, you can make the mosaic dataset portable enough that if the reason you wanted to use relative paths was so you could move the data around or to other machines, you can. Just need to repair the paths.

So now, until we can actually track down the original data, we at least have a usable, archive of the imagery we wanted to preserve and have a way to access it in the field in a non-connected environment.