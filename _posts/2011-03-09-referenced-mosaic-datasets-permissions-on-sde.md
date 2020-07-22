---
id: 424
title: Referenced Mosaic Datasets Permissions on SDE
date: 2011-03-09T09:08:27-06:00
author: Matt
layout: post
guid: http://maprantala.com/?p=424
permalink: /2011/03/09/referenced-mosaic-datasets-permissions-on-sde/
geo_latitude:
  - "0"
geo_longitude:
  - "0"
geo_accuracy:
  - "0"
geo_public:
  - "1"
categories:
  - ArcGIS 10
tags:
  - arc sde
  - arcgis 10
  - ESRI
  - referenced mosaic datasets
  - sde
---
While I&#8217;m enjoying the functionality of the new [Referenced Mosaic Dataset](http://help.arcgis.com/en/arcgisdesktop/10.0/help/index.html#//00170000008p000000.htm) that have been introduced in ArcGIS10, something that I stumbled over recently was administering the privileges to a referenced mosaic dataset.

A referenced mosaic dataset is a raster datatype that uses a mosaic dataset as a base.  For example, we loaded a series of DEMs into a mosaic dataset and then created a referenced mosaic dataset that does the meters to feet conversion for us.  This saved us from having to process each DEM before loading it.  We also created a referenced mosaic dataset hillshade from the original meter data.

The problem I came across, and this was more my oversight than anything, but I had a heck of a time trying to grant privileges to the referenced mosaic dataset.  The solution was actually very simple&#8211;I needed to also grant access to the mosaic dataset that was being used as the base.

That makes some sense, you should not be able to grant access to a dataset (the referenced mosaic dataset) that would inadvertently be exposing data (the base mosaic dataset) that a user does not have access to.  This, however, conflicts a bit in that the permissions on individual rasters that make up a mosaic raster dataset are ignored.

<div id="geo-post-424" class="geo geo-post" style="display: none">
  <span class="latitude"></span><span class="longitude"></span>
</div>