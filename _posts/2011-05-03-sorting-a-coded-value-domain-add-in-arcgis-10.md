---
id: 522
title: Sorting a Coded-Value Domain Add-In (ArcGIS 10)
date: 2011-05-03T03:33:01-05:00
author: Matt
excerpt: 'An ArcGIS 10 version of a tool to sort your Coded-Value Domains.  Includes source code and a ready-to-install esriaddin.'
layout: post
guid: http://maprantala.com/?p=522
permalink: /2011/05/03/sorting-a-coded-value-domain-add-in-arcgis-10/
geo_public:
  - "1"
geo_accuracy:
  - "1447"
geo_address:
  - 8341-8345 Suffolk Dr, Chanhassen, MN 55317, USA
geo_longitude:
  - "-93.55073"
geo_latitude:
  - "44.852994"
categories:
  - ArcGIS
  - ArcObjects
  - ArcScripts
  - ArcSDE
  - ESRI
  - VB.Net
tags:
  - arcgis
  - arcgis 10
  - ArcObjects
  - ArcSDE
  - coded-value domain
  - GIS
  - VB 6
  - vb.net
---
I am working on an data-entry application to edit feature classes that contain several coded-value-domains. The problem with some of the domains, however, is that some entries have been added after the initial creation.  So the first 25 entries are in alphabetical order and there are some stragglers at the end that are in the order they were appended.

This can be confusing for users&#8211;they go to select &#8220;Milli Vanilli&#8221; and look between &#8220;Madonna&#8221; and &#8220;Motley Crue&#8221; but can not find their favorite band there&#8211;they have to go to the end of the list to find their selection.

In the past, I have gone through the tedious process of exporting the domain to a table, sorting the table, removing the domain from the necessary field(s), deleting the domain, re-importing the table back in a new domain and finally re-applying the domain to the necessary field(s). Let&#8217;s just say I didn&#8217;t do this until someone asked a few times and I didn&#8217;t have anything more exciting&#8211;like a root canal&#8211;I could busy myself with.

But this new application contains more domains than any of other datasets so it was time to find a better solution. ESRI does have a [Domain Sort](http://edndoc.esri.com/arcobjects/9.2/CPP_VB6_VBA_VCPP_Doc/COM_Samples_Docs/Geodatabase/Schema_Creation_and_Management/Sort_a_domain/e826c5a8-9740-4f0b-86b6-d3b834735574.htm) Developer Sample.  It, however, did [not play nice](http://edndoc.esri.com/arcobjects/9.2/CPP_VB6_VBA_VCPP_Doc/COM_Samples_Docs/Geodatabase/Schema_Creation_and_Management/Sort_a_domain/e826c5a8-9740-4f0b-86b6-d3b834735574.htm) with ArcGIS 10.

So I went ahead and update it from VB 6 to VB.Net/ArcObjects 10.  I made an [Add-In](http://dl.dropbox.com/u/22241283/NodeDangles/DomainSorter.esriAddIn) that can be installed by downloading the .esriaddin file and double-clicking on it.  The [source code](http://dl.dropbox.com/u/22241283/NodeDangles/20120321_DomainSort.zip) is also available.

This will add an ArcCatalog Toolbar that can be added by going to Customize-Toolbars-Domain Sorter Toolbar.

[<img class="aligncenter size-full wp-image-540" title="Sort Domain-Add Toolbar" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2011/04/ds_1.png?resize=379%2C239" alt="" width="379" height="239" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2011/04/ds_1.png)

This will add a toolbar with one button.  The button enables whenever you select a geodatabase with at least one coded-value domain.

[<img class="aligncenter size-full wp-image-539" title="Domain Sorter Button" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2011/04/ds_2.png?resize=214%2C88" alt="" width="214" height="88" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2011/04/ds_2.png)

This brings up a Windows form that lets you sort any domain by either the code or description, ascending or descending.  Once you hit &#8220;OK&#8221; it re-sorts your domain.

[<img class="aligncenter size-full wp-image-538" title="Domain Sorter Dialog" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/04/ds_3.png?resize=442%2C416" alt="" width="442" height="416" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/04/ds_3.png)

The only problem I have had is that only the owner of a domain is allowed to edit it on an SDE geodatabase.

But other than that, the button allows you to easily keep your domains sorted.

<small>http://edndoc.esri.com/arcobjects/9.2/CPP_VB6_VBA_VCPP_Doc/COM_Samples_Docs/Geodatabase/Schema_Creation_and_Management/Sort_a_domain/e826c5a8-9740-4f0b-86b6-d3b834735574.htm</small>

<div id="geo-post-522" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>