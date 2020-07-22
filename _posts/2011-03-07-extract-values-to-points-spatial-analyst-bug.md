---
id: 402
title: Extract Values to Points (Spatial Analyst) Bug
date: 2011-03-07T08:37:33-06:00
author: Matt
excerpt: Description of an ArcGIS Spatial Analyst bug that occurs when the "Extract Values to Points" tool comes across a point with Null geometry.
layout: post
guid: http://maprantala.com/?p=402
permalink: /2011/03/07/extract-values-to-points-spatial-analyst-bug/
twitter_cards_summary_img_size:
  - 'a:7:{i:0;i:819;i:1;i:313;i:2;i:2;i:3;s:24:"width="819" height="313"";s:4:"bits";i:8;s:8:"channels";i:3;s:4:"mime";s:10:"image/jpeg";}'
categories:
  - ArcGIS
  - ArcGIS 10
  - Bugs
  - Bugs
  - ESRI
tags:
  - arcgis
  - arcgis 10
  - arcgis bug
  - ArcGIS Spatial Analyst
  - ArcObjects
  - arctoolbox
  - ESRI
  - esri bugs
  - Extract Multi Values to Points
  - Extract Values to Points
  - Null geometry
  - spatial analyst
---
One of the Spatial Analyst tools we often use in ArcGIS is the &#8220;[Extract Values to Points](http://help.arcgis.com/en/arcgisdesktop/10.0/help/index.html#//009z0000002t000000.htm)&#8221; tool.  This allows us to take a point file (well locations in our case) and attach a value (elevations) from a raster image (a DEM) to each point.

[<img class="aligncenter size-full wp-image-403" title="Extract Values To Points" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2011/03/extractvaluestopoints.jpg?resize=630%2C240" alt="" width="630" height="240" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2011/03/extractvaluestopoints.jpg)

Today I was running it for the first time against an Image Service we recently published and I received a warning message,&#8221;WARNING 000957: Skipping feature(s) because of NULL or EMPTY geometry&#8221;.  But the script seemed to run and the final results said &#8220;Succeeded&#8221; so I thought it was probably fine.  But as I double-checked, I realized the results were wonky.

Turns out that I had two records with Null geometry in my point file of 397 records.  These two records threw the above error but actually had a value in the [Rastervalu] field.  Turns out all 397 records had values.  These two records were consecutive&#8211;let&#8217;s say the 100th and 101st records in my shapefile.  What happened is record 100 got the value for record 102, record 101 get the value for 103, record 102 (which has valid geometry) had the value for 104.  This pattern, each record having the value for the record 2 place after it, continued until record 396 which had the value for record 397.  Record 397 also had the value for 397.  So the final three records all had the value for the final record.

What I would have expected would be for the two records with Null Geometry to have null values in the [Rastervalu] field and the rest of the records to have the correct values.  Despite the warning, it is very misleading for all the records to end up with a value.

I have a simplified example below.  I made a point shapefile with four records.  The first, third, and fourth  records have valid geometries; the second has Null geometry.  The second record ends up with the value for the third record.  The third record, has the value for the fourth.  The fourth record being the last record, ends up with the last valid value, which was its own.

[<img class="aligncenter size-full wp-image-405" title="Test Results" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/03/results.jpg?resize=528%2C327" alt="" width="528" height="327" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/03/results.jpg)

The results that I would have hoped for would be for the third record to have a Null value.

The way I envision what is occurring behind the scenes is this:  the process makes a list (more of a stack in programming terms) of result values as it processed the points but just assumes that every record will return a value so it does not track which value goes with which shape.

When it reached the two null geometries, it threw an error but continued on.  It did not add a value for these records to the stack of values&#8211;when it comes across records with valid geometry but do not intersect the raster it adds a psuedo-null value of (-9999) to the stack.  After it processed all the records it had 395 values in the stack.  It then went, one-by-one through the stack and populated the records in the output shapefile, the first record got the first value in the stack, the second record got the second value, the 100th record got the 100th value (which came from the location of the 102nd record) and so on.  At the end, the final two records received the last valid value.

This final behavior&#8211;using the last valid value&#8211;corresponds a bit to a behavior we&#8217;ve seen with ArcObjects in general.  When iterating through a table, if a field is Null for a specific row, the value from the last non-Null value for that field is often returned.

I&#8217;m in the process of submitting a bug to ESRI.  I&#8217;m not sure if this existed prior to ArcGIS 10.0 (I&#8217;m guessing it did) or if it occurs in other processes (I&#8217;m guessing it does).  I did find out that the &#8220;[Extract Multi Values to Points](http://help.arcgis.com/en/arcgisdesktop/10.0/help/index.html#//009z0000002s000000.htm)&#8221; works as expected.  I&#8217;m guessing it is because unlike the &#8220;Extract Values to Points&#8221; which creates a new shapefile, this tool appends fields to the existing shapefile and presumably processes records one-by-one without putting the results in a virtual stack.  The &#8220;Extract Multi Values to Points&#8221; tool also does not throw any warnings.

[<img class="aligncenter size-full wp-image-406" title="Extract Multi Values to Points" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2011/03/same.jpg?resize=473%2C478" alt="" width="473" height="478" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2011/03/same.jpg)

[  
](http://maprantala.com/wp-content/uploads/2011/03/extractvaluetopoints.jpg)