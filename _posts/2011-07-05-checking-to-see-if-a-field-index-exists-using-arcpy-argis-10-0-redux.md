---
id: 581
title: Checking to see if a Field Index Exists Using Arcpy (ArGIS 10.0) redux
date: 2011-07-05T12:53:34-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=581
permalink: /2011/07/05/checking-to-see-if-a-field-index-exists-using-arcpy-argis-10-0-redux/
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
  - arcpy
  - ArcScripts
  - ESRI
  - geoprocessing
  - Python
tags:
  - arcgisscripting
  - arcpy
  - argis 10
  - ESRI
---
I&#8217;ve previously posted python code to check if a field index exists for both [ArcGIs 9.3](http://maprantala.com/2011/01/27/checking-to-see-if-a-field-index-exists-using-python/) and [ArcGIS 10.0](http://maprantala.com/2011/02/21/checking-to-see-if-a-field-index-exists-using-arcpy-argis-10-0/).

Recently I have been working on a process that was using this code but it was not working because it looks for an index with a specific name.  It was not working in this case because the name of the indexes was getting incremented as they were being created.  For example, I was building an index on the table C5ST, field RelateId ([C5IX].[Relateid]) named I\_C5IX\_RelateId.  That worked fine until we switched our process so now we keep multiple versions of some tables, each with a date-based suffix.

We now have tables name C5St\_20110625 and C5St\_20110626&#8211;the Index-name scheme, however was still creating I\_C5IX\_RelateId and it worked great on the first one.  But when it created the second one, even on a different table, it was automatically name I\_C5IX\_RelateId\_2 even though the name I\_C5IX_RelateId was used when trying to create the index.

Before generating relates, our code checks to see if the key fields are indexed, and if they are not, builds  an index.  Because of the naming situation, multiple, duplicate indexes were being created.  Probably not too harmful but it is a little messy.

So I re-wrote the code so that you pass the function the table name and field name that you want to check and it checks to see if there is an index existing for that field and return a Boolean.  The one little wrinkle I put in is to account for indexes that span multiple fields&#8211;the &#8221; if (iIndex.fields[0].Name.upper() == fieldname.upper()):&#8221; statement is checking the index to see if it is on a single field or multiple fields.

&nbsp;

> def fieldHasIndex(tablename,fieldname):  
> if not arcpy.Exists(tablename):  
> return False
> 
> tabledescription = arcpy.Describe(tablename)
> 
> for iIndex in tabledescription.indexes:  
> if (len(iIndex.fields)==1):  
> if (iIndex.fields[0].Name.upper() == fieldname.upper()):  
> return True
> 
> return False

<div id="geo-post-581" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>