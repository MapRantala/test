---
id: 666
title: Domain Sorter Add-In Version 1.1
date: 2012-03-21T18:19:03-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=666
permalink: /2012/03/21/domain-sorter-add-in-version-1-1/
categories:
  - ArcGIS 10
  - ArcObjects
  - ArcScripts
  - Bugs
  - Bugs
  - ESRI
  - VB.Net
tags:
  - ArcCatalog
  - coded-value domain
  - domain
  - vb.net
---
Almost [a year ago](http://maprantala.com/2011/05/03/sorting-a-coded-value-domain-add-in-arcgis-10/), I updated ERSI&#8217;s [Domain Sort code for VB 6](http://edndoc.esri.com/arcobjects/9.2/CPP_VB6_VBA_VCPP_Doc/COM_Samples_Docs/Geodatabase/Schema_Creation_and_Management/Sort_a_domain/e826c5a8-9740-4f0b-86b6-d3b834735574.htm) to work with ArcGIS 10. Recently, I had a comment that this Add-In caused ArcCatalog to explode if you had an open OLE connection. When I tested it, it turned out the reports were accurate.

[<img class="aligncenter size-full wp-image-667" title="ArcCatalog" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2012/03/arccatalog.png?resize=568%2C583" alt="" width="568" height="583" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2012/03/arccatalog.png)

I got around to adding in a Try-Catch around the offending chunk of code & it is now better than ever. You can download just the [Add-In](http://dl.dropbox.com/u/22241283/NodeDangles/DomainSorter.esriAddIn) or the [Add-In with source code](http://dl.dropbox.com/u/22241283/NodeDangles/20120321_DomainSort.zip) or get it from [ESRI&#8217;s ArcGIS Resource Center](http://resources.arcgis.com/gallery/file/arcobjects-net-api/details?entryID=B5E1BB12-1422-2418-34B7-D3A6908560FA).