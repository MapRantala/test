---
id: 599
title: ArcGIS Add-In Custom Mouse Cursor
date: 2011-07-19T20:47:03-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=599
permalink: /2011/07/19/arcgis-add-in-custom-mouse-cursor/
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
  - ArcObjects
  - ArcScripts
  - VB.Net
tags:
  - arcgis 10
  - ArcMap
  - ArcObjects
  - vb.net
---
I was working on a project and wanted my own custom mouse cursor and did not easily find a way to make your own in ESRI&#8217;s instructions.  But, once you know how to do it, it is pretty easy.  In Visual Studio, Add a New Item:

[<img class="aligncenter size-full wp-image-600" title="Add Resource" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2011/07/addresource.png?resize=405%2C237" alt="" width="405" height="237" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2011/07/addresource.png)

Add a Cursor File:

[<img class="aligncenter size-full wp-image-602" title="SelectCursor" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/07/selectcursor.png?resize=614%2C372" alt="" width="614" height="372" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/07/selectcursor.png)

You can edit your cursor with the editor program in Visual Studio.  Once you satisfied with how it looks, make sure that the Build Action on the cursor is &#8220;Embedded Resource&#8221;.

[<img class="aligncenter size-full wp-image-601" title="Embedded" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2011/07/embedded.png?resize=605%2C235" alt="" width="605" height="235" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2011/07/embedded.png)

Then you can set your cursor with two lines of code. Not that my cursor is in my QDI.QdiAddIn Namespace:

<pre>Dim pCursorStream As System.IO.Stream = Me.GetType.Assembly.GetManifestResourceStream("QDI.QdiAddIn.NewCursor.cur")
MyBase.Cursor = New System.Windows.Forms.Cursor(pCursorStream)</pre>

<div id="geo-post-599" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>