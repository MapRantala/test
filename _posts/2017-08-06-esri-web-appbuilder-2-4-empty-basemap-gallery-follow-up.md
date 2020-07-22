---
id: 4704
title: Esri Web AppBuilder 2.4, Empty Basemap Gallery Follow-Up
date: 2017-08-06T10:12:07-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=4704
permalink: /2017/08/06/esri-web-appbuilder-2-4-empty-basemap-gallery-follow-up/
categories:
  - Bugs
  - Web AppBuilder
tags:
  - Bugs
  - ESRI
  - Web Appbuilder
---
A while ago I <a href="http://maprantala.com/2017/04/10/esri-web-appbuilder-2-4-empty-basemap-gallery/" target="_blank" rel="noopener">posted </a>a work-around for a problem I was having with a <a href="http://www.esri.com/software/web-appbuilder" target="_blank" rel="noopener">Web AppBuilder</a> application. Working with Esri tech support, we determined what I was doing to cause the problem.

In my config.json, I was using a relative path to the proxy. Note that I had the proxy as a separate application at the root level of our domain because I intended to have a shared proxy for all of our applications instead of individual ones. So the WAB application would be using http://ourarcgisdomain.com/proxy/proxy.ashx.

<pre>"httpProxy": {
	"useProxy": true,
	"alwaysUseProxy": true,
    "url": "/proxy/proxy.ashx",
    "rules": []
  }
</pre>

Well, that was causing the hiccup in the <a href="http://doc.arcgis.com/en/web-appbuilder/create-apps/widget-basemap.htm" target="_blank" rel="noopener">Basemap Gallery Widget</a>. I simply modified the url parameter to have the full path of our domain, undid my custom code, and the Basemap Gallery Widget was happy.

<pre>"httpProxy": {
     "useProxy": true,
     "alwaysUseProxy": true,
     "url": "http://ourarcgisdomain.com/proxy/proxy.ashx",
     "rules": []
   }
</pre>

Score one for Esri Tech Support.

Although, in my defense the <a href="https://developers.arcgis.com/web-appbuilder/api-reference/app-configuration.htm" target="_blank" rel="noopener">documentation on the configuring the proxy</a> is silent on the pathing.

&nbsp;