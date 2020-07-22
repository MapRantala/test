---
id: 933
title: 'Friday Fave: Geodatabase Geek'
date: 2014-05-02T05:02:12-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=933
permalink: /2014/05/02/friday-fave-geodatabase-geek/
categories:
  - ArcGIS
  - ArcGIS 10
  - Friday Fave
---
This [Friday Fave](http://maprantala.com/category/friday-fave/) is more for utility than pleasure.

Unfortunately, I have been working to determine why my views and query layers perform so much worse than directly accessing my feature class.

My Googling led me to <a href="http://gdbgeek.wordpress.com/" target="_blank">Geodatabase Geek</a>, by Trevor Hart, <a href="http://www.eagle.co.nz/" target="_blank">Eagle Technology Group Ltd</a>.  Trevor has some real good information about Geodatabases and also  gave a good lightening talk on [Usage Reporting on ArcGIS 10.1 for Server ](http://video.esri.com/watch/2481/usage-reporting-on-arcgis-101-for-server)at the 2013 ESRI International Developer&#8217;s Conference.

One tool he pointed out was <a href="https://www.arcgis.com/home/item.html?id=a269d03aa1c840638680e2902dadecac" target="_blank">Mxdperfstat</a> for benchmarking the performance of your MXD. Trevor used it to compare the performance of a <a href="http://gdbgeek.wordpress.com/2014/01/15/feature-class-vs-query-layer-vs-spatial-view/" target="_blank">Feature Class vs Query Layer vs Spatial View</a>. While the official version is available for ArcGIS 9.3 through 10.2, I do want to point out <a href="http://www.husseinnasser.com/2013/04/mxdperfstat-101-download-mxd.html" target="_blank">Hussein Nasser&#8217;s 10.1 version</a> which he put out before the official 10.1 version came out (it&#8217;s not really a version, more of a work-around but I like his ingenuity).

My results were significantly different on our 10.0 database server, the spatial view I was testing was much slower.  The query for both the spatial view and query layer was simply &#8220;Select * from _featureclass_&#8221;

So not sure what to make of the performance yet, I&#8217;ve got a spatial index made so not sure what else I can try.<figure id="attachment_936" aria-describedby="caption-attachment-936" style="width: 597px" class="wp-caption alignnone">

[<img class=" wp-image-936" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2014/05/MGSDB1-300x136.png?resize=597%2C271" alt="ArcSDE 10.0 Performance" width="597" height="271" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2014/05/MGSDB1.png)<figcaption id="caption-attachment-936" class="wp-caption-text">ArcSDE 10.0 Performance</figcaption></figure> 

&nbsp;