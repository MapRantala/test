---
id: 328
title: 'Walkthrough: Building custom UI elements using add-ins (ArcObjects .NET 10 SDK)'
date: 2011-02-15T17:05:55-06:00
author: Matt
excerpt: "ESRI's Add-In walkthrough for building an ArcMap Add-In comes with minor, fixable, bugs."
layout: post
guid: http://maprantala.com/?p=328
permalink: /2011/02/15/walkthrough-building-custom-ui-elements-using-add-ins-arcobjects-net-10-sdk/
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
  - ArcMap
  - ArcObjects
  - ArcScripts
  - Bugs
  - Bugs
  - ESRI
  - Programming
  - VB.Net
tags:
  - .Net
  - arcgis
  - arcgis 10
  - ArcObjects
  - Bugs
  - vb.net
---
I was working my way through this ESRI [Walkthrough: Building custom UI elements using add-ins (ArcObjects .NET 10 SDK)](http://help.arcgis.com/en/sdk/10.0/arcobjects_net/conceptualhelp/index.html#//0001000001ms000000).  And came across a couple minor errors that I had to correct during the process.

First, while implementing the OnClick() code for ZoomToLayer.vb, Visual Studio gave me a &#8220;Name &#8216;ArcMap&#8217; is not declared.&#8221; error.

[<img class="alignnone size-full wp-image-331" title="ArcMap Not Declared" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/02/arcmapnotdeclared.jpg?resize=630%2C131" alt="" width="630" height="131" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/02/arcmapnotdeclared.jpg)

In the walk-through, they mention that the ArcMap method of your class.  For me, however, it appeared under the .My method.  Not sure if this is something specific to my set-up or, as I&#8217;m guessing, something that got changed after the first documentation was created and the final libraries published.

[<img class="alignnone size-full wp-image-332" title="Object Browser" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2011/02/objectbrowser.jpg?resize=520%2C264" alt="" width="520" height="264" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2011/02/objectbrowser.jpg)

The fix is just adding  &#8220;My.&#8221; to the namespace in this line:

<pre>ZoomToActiveLayerInTOC(TryCast(ArcMap.Application.Document, IMxDocument))</pre>

To get this:

<pre>ZoomToActiveLayerInTOC(TryCast(My.ArcMap.Application.Document, IMxDocument))</pre>

When I added the code for AddGraphics.vb, I got 8 errors.  There was essentially two errors, repeated four times.  I took a screen shot after fixing the first error pair:

[<img class="alignnone size-full wp-image-330" title="Add Graphics Error List" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2011/02/errorlist.jpg?resize=630%2C161" alt="" width="630" height="161" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2011/02/errorlist.jpg)

The fixes in this case was also to use the complete name space path.  Examples:

Change this:

<pre>(geometry.GeometryType) = esriGeometryType.esriGeometryPoint Then</pre>

To this:

<pre>If (geometry.GeometryType) = ESRI.ArcGIS.Geometry.esriGeometryType.esriGeometryPoint Then)</pre>

And change this:

<pre>simpleMarkerSymbol.Style = esriSimpleMarkerStyle.esriSMSCircle</pre>

To this:

<pre>simpleMarkerSymbol.Style = ESRI.ArcGIS.Display.esriSimpleMarkerStyle.esriSMSCircle</pre>

Overall, the walk-through is very well done, just a couple minor tweaks.  I am now working my way through modifying an existing solution&#8211;one that included seven projects&#8211;to see if I can create an ArcGIS 10 Add-In.

<div id="geo-post-328" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>