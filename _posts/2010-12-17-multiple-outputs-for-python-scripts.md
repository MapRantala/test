---
id: 257
title: Multiple outputs for Python scripts
date: 2010-12-17T15:19:38-06:00
author: Matt
layout: post
guid: http://maprantala.com/?p=257
permalink: /2010/12/17/multiple-outputs-for-python-scripts/
categories:
  - ArcScripts
  - ESRI
  - geoprocessing
  - Misc
  - Python
tags:
  - arcgis
  - ArcScripts
  - geoprocessing
  - output messages
  - python
---
Related to my [post](http://maprantala.com/2010/12/13/launching-a-python-script-with-parameters-3-methods/) on how I enable a script to accept parameters from different sources, I also often set up pythons scripts to output information a variety of ways.  This is largely due to the fact that some are called by ArcToolbox scripts.  Running in ESRI&#8217;s domain, these scripts need to send the output through the arcgisscripting object but if you are running the python outside the ArcGIS framework, you can just print.

If you assume one output method but then run your code in the opposite framework, you don&#8217;t get to see all the pretty little messages.  What I do is create a simple little routine that broadcasts the message both ways.  This is probably an obvious solution but took a few cases before I went ahead and started implementing it.

<pre>gp = arcgisscripting.create()

#This will print both to the geoprocessing window or Python output window
def gpprint(inmessage):
 gp.addmessage(inmessage)
 print inmessage

&lt;Code to do stuff&gt;

#Ok, I want to send a message:
gpprint('Hello, sailor!')
</pre>