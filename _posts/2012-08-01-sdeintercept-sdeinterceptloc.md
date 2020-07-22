---
id: 744
title: 'SDEINTERCEPT &#038; SDEINTERCEPTLOC'
date: 2012-08-01T07:17:41-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=744
permalink: /2012/08/01/sdeintercept-sdeinterceptloc/
categories:
  - arcpy
  - ArcSDE
  - Bugs
  - ESRI
  - geoprocessing
tags:
  - ArcSDE
  - arcsde 10
  - ESRI
---
Awhile ago, I had a ArcSDE problem that required [ESRI technical support](http://support.esri.com/en/) to help trouble-shoot. The problem was odd but was resolved by rebooting the server.  
During the process, though, the support person had me set a couple of environment variables for logging SDE activity on the client machine.

The settings were SDEINTERCEPT and SDEINTERCEPTLOC.

From [ESRI&#8217;s Help](http://help.arcgis.com/en/arcgisdesktop/10.0/help/index.html#//002n00000018000000), SDEINTERCEPT specifies what activity to log and SDEINTERCEPTLOC specifies where to save the log files.

I recently deleted the directory I made for the log files but did not remove the variables and I noticed that one of my python scripts reported a weird error (but continued to run, I think). I tracked it back to these variables and realized what I had done.

Googling SDEINTERCEPTLOC did lead me to some helpful information like:

The [SDEINTERCEPT blog](http://sdeintercept.wordpress.com/2011/11/22/sde-user-permissions/) where Ken posts ArcSDE help.

This [ESRI post](http://blogs.esri.com/esri/arcgis/2008/09/05/digging-deeper-troubleshooting-geoprocessing-errors-when-using-arcsde-data/) about troubleshotting geoprocessing problems.

And this [ESRI technical](http://support.esri.com/en/knowledgebase/techarticles/detail/35704) article about diagnosing ArcSDE Connections.