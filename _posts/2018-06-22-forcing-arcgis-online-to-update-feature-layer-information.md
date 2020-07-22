---
id: 4782
title: Forcing ArcGIS Online to Update Feature Layer Information
date: 2018-06-22T20:26:28-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=4782
permalink: /2018/06/22/forcing-arcgis-online-to-update-feature-layer-information/
categories:
  - ArcGIS Online
  - Bugs
  - ESRI
tags:
  - ArcGIS Online
  - Bugs
  - ESRI
---
Because we use ArcGIS Online Web Maps in our daily work-flows, I created an AGO group that contains Feature Layers that are links to our self-hosted ArcGIS Server services. This way, users can quickly add data from our ArcGIS Server Site and not have to look up URLs/Usernames/Passwords (I&#8217;ve save the username/password as part of the ArcGIS Online service when appropriate).

But I&#8217;ve had a problem where if I change the service, the changes are not reflected in the AGO Feature Layer for that service and I have to monkey about to get the changes to migrate to the maps.

Finally annoyed with doing that for the Xth time, I decided to see if I could get the feature service information to refresh automatically. Documentation search found nothing; call in to tech support&#8211;still waiting. While experimenting, however, I found that newer Feature Layer I added this way do actually update when I made changes. WTH?!

Using <a href="https://ago-assistant.esri.com/#" target="_blank" rel="noopener">ArcGIS Online Assistant</a>, something caught my eye. My older Feature Layers had a Description and Data but the newer (automatically updating ones) do not.

[<img class="alignnone size-full wp-image-4784" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2018/06/Data-1.jpg?resize=918%2C306" alt="" width="918" height="306" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2018/06/Data-1.jpg)

Was this the answer&#8211;the 912 line JSON file for this service had all the information about the Feature Service. So it seems like the information about my Feature Service probably got saved when I create the Feature Layer and this cache was used later instead of gathering new information. Now that I had a suspected reason why this was occurring, I needed to figure out how to either delete or force a refresh of the cache. There&#8217;s no delete option next to the Data section so I figured I would empty the content and see if that would work.

<span style="color: #ff0000;">WARNING: Unsupported Technique Ahead!! Proceed at Own Risk&#8211;Test First with a Copy of Data!!!</span>

<span style="color: #000000;">Using AGO Assistant, I copied the Feature Layer that did not recognize when I made changes to the Feature Service and then made a map with that new Feature Layer so that I could test against a copy of the data. Once I had those, I went to the Copy of my Feature Layer in AGO Assistant and deleted almost everything in the Data section and saved that. (Note that it despite saying it saved my edits when I deleted everything, the contents were still there so you have to leave something in there).</span>

<span style="color: #000000;"> <a href="https://i1.wp.com/maprantala.com/wp-content/uploads/2018/06/almostEmptyJSON.png"><img class="alignnone size-full wp-image-4787" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2018/06/almostEmptyJSON.png?resize=514%2C128" alt="" width="514" height="128" data-recalc-dims="1" /></a></span>  
Once I went and looked at the Feature Layer and it showed the layer I had previously added that was not showing up. And when added to a New map, it reflected all my changes. The existing map, however, still had previous settings&#8211;a new field, for example, did not appear.

So I found a partial fix&#8211;the Feature Layer hopefully will not cache any information about the Feature Service so when I add it to a map, the most recent settings will be used. Will still need to update maps but at least I have a Feature Layer now that reflects the most recent changes to the Feature Service.