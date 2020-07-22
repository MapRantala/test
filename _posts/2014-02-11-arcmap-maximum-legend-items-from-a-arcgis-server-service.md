---
id: 801
title: ArcMap Maximum Legend Items from a ArcGIS Server service
date: 2014-02-11T06:02:42-06:00
author: Matt
excerpt: Adjusting your registry if a legend for an ArcGIS service layer does not display in ArcMap.
layout: post
guid: http://maprantala.com/?p=801
permalink: /2014/02/11/arcmap-maximum-legend-items-from-a-arcgis-server-service/
publicize_twitter_user:
  - MapRantala
publicize_twitter_url:
  - http://t.co/zrII1hqSOi
publicize_linkedin_url:
  - 'http://www.linkedin.com/updates?discuss=&scope=24792516&stype=M&topic=5838975407864422400&type=U&a=S3r3'
categories:
  - ArcGIS
  - ArcGIS Server
  - ArcMap
---
Recently we took a call from a user who could not see the legend for one of the feature classes in one of our services. (Precambrian Bedrock in http://mgsweb2.mngs.umn.edu/arcgis/services/state/mnbdrkgeology )

After trying some standard things&#8211;restarting the service, checking the source .MXD&#8211;I turned to The Google Machine and quickly found help from ESRI: http://support.esri.com/zh-cn/knowledgebase/techarticles/detail/33741 .

Turns out the default number of legend items ArcMap will display from an ArcGIS Server map service layer is 100 and we had 102 in the problematic layer. (I plead innocence, blame the geologist for needing that many categories).

The solution was to edit the Windows registry and change the setting for &#8220;Maximum Legend Count&#8221; from 100 to something higher than 102. After doing that (see path note below) and restarting ArcMap, the legend showed for us.

**Path Details**  
The path turned out to be a little different on Windows 7 than the paths indicated in the help article.  
ESRI indicated that the path varied by ArcGIS version:

  * ArcGIS 9.3.x: HKEY\_CURRENT\_USER > Software > ESRI > MapServerLayer
  * ArcGIS 10.0: HKEY\_CURRENT\_USER > Software > ESRI > Desktop10.0 > ArcMap > Server > MapServerLayer
  * ArcGIS 10.1: HKEY\_CURRENT\_USER > Software > ESRI > Desktop10.1 > ArcMap > Server > MapServerLayer

I actually changed this in a number of locations&#8211;presumably once for each user profile on my machine. Each followed a pattern something like:

HKEY_USERSS-1-5-23412341234-123412342134-12341234123-1302SoftwareESRIArcMapServerMapServerLayer

**Extra Information**

Out of cursiousity, I wondered if the 100 item limit was a per **service** or per **layer** limitation so I set my limit at 103. Because the service has several other layers, the total number of layers in the legend was about 130. Everything drew OK so it appears the limit applies per layer.