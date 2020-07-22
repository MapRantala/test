---
id: 30
title: ArcMap-Excel Join Error
date: 2010-05-28T02:02:37-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=30
permalink: /2010/05/28/arcmap-excel-join-error/
categories:
  - ArcGIS
  - ArcMap
  - Bugs
  - ESRI
tags:
  - ArcMap
  - Bugs
  - ESRI
  - Excel
  - Join
---
Came across a new ArcMap bug today. A staff member was trying to join an Excel 2007 file (.xlsx) to a point shapefile in ArcMap and it did not appear to be working. She only had Office 2003 on her machine so I tried it on my spanking new machine that has 2007 installed. Same result&#8211;the fields get appended but they are all blank.

I discovered, however, that when I had the table open, if I switched form showing ALL records to only the selected, Poof!!! The values filled in. From there I was able to successfully export the data to a new shapefile that always showed the values.

I have an inkling that the file name, which contained space and other special characters, may have been a factor.

**August 11, 2010 Addendum:**

I was just working with some more and, after joining a table to an Excel table, having all the fields look blank, I discovered that if I close and then re-open the table all looks good&#8211;all fields have values.

Another problem I had is that even after successfully joining a table to the Excel table and creating an XY Event Theme from it, I was unsuccessful in exporting it to either a shapefile or a personal geodatabase feature class.  My personal opinion at this point would be to convert the Excel file to a different format before working with it in ArcGIS.