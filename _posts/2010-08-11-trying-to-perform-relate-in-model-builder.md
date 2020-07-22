---
id: 169
title: 'Trying to perform &#8220;relate&#8221; in Model Builder'
date: 2010-08-11T17:31:21-05:00
author: Matt
excerpt: A quick illustration of using the Make Table View tool to simulate doing a relate in Model Builder.
layout: post
guid: http://maprantala.com/?p=169
permalink: /2010/08/11/trying-to-perform-relate-in-model-builder/
categories:
  - ArcGIS
  - geoprocessing
  - Model Builder
  - Python
---
I was using Model Builder (ugh!) to select records in one table (CWI.C5ST) that relate to a subset of records ([BHGEOPHYS] = &#8216;Y&#8217;) in another table (CWI.C5IX).  There is not an existing tool for doing this in ArcGIS.  I did find a [post](http://forums.esri.com/Thread.asp?c=93&f=1728&t=257179&g=1) by Layne Seely in [ArcForums](http://forums.esri.com/) titled &#8220;trying to perform &#8220;relate&#8221; in Model Builder.&#8221; that led me to the Make Table View under Data Management Tools-Make Table View and even had  the basic syntax I was looking for (if it hadn&#8217;t I probably would have guessed that it would not allow subqueries).

Since I always like to see exactly the syntax someone uses, I decided to show the syntax I used in case that helps anyone else.<figure id="attachment_171" aria-describedby="caption-attachment-171" style="width: 615px" class="wp-caption alignnone">

[<img class="size-medium wp-image-171" title="tableView" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2010/08/tableview.jpg?resize=615%2C392" alt="" width="615" height="392" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2010/08/tableview.jpg)<figcaption id="caption-attachment-171" class="wp-caption-text">Sample of a Table View using a subquery</figcaption></figure> 

It does require that the tables reside in the same workspace, which is a reasonable limitation that I can live with.  Another drawback is that if you export the model to a python script, the command can get pretty long, especially if you have many fields.