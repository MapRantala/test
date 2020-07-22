---
id: 394
title: Instantiating Add-In Objects at Load Time
date: 2011-03-02T15:18:14-06:00
author: Matt
excerpt: Details on how to disable an Add-In control at load time.
layout: post
guid: http://maprantala.com/?p=394
permalink: /2011/03/02/instantiating-add-in-objects-at-load-time/
categories:
  - ArcGIS 10
  - ArcObjects
  - ESRI
  - VB.Net
tags:
  - arcgis 10
  - arcgis add-in
  - ArcObjects
  - vb.net
---
In migrating a toolbar consisting of a button and a couple of tools for use in ArcMap 10, I decided to take advantage of the ease of deployment enabled by add-ins which was introduced in 10.0.  So far, I&#8217;m loving the functionality.

One thing, however, that I have to figure out is that the controls are not instantiated until they are clicked on.  One of the results is that the controls, by default, are enabled.  This is not the functionality I wanted.

I found the solution in ESRI&#8217;s topic, [Advanced add-in concepts (ArcObjects .NET 10 SDK)](http://help.arcgis.com/en/sdk/10.0/arcobjects_net/conceptualhelp/index.html#/Advanced_add_in_concepts/0001000004n7000000/) in the Delayed loading section:

<p style="padding-left:30px;">
  &#8220;<strong></strong>By default, the assemblies associated with add-in buttons and tools are not loaded until the corresponding item on a toolbar or menu is clicked by the user. This behavior helps conserve application memory and other resources. Since the enabled state of an add-in button or tool is controlled by the OnUpdate method within code, the button or tool will initially appear enabled. If you need tighter control over the initial enabled state of a button or tool, you need to override the default behavior and force the item to load at startup by setting the onDemand Extensible Markup Language (XML) attribute to false.&#8221;
</p>

The verbiage, &#8220;setting the onDemand Extensible Markup Language (XML) attribute to false&#8221; was a bit non-specific to me.  I guessed right, however, that they were referring to the control definiton in the Command section of Config.esriaddinx.  In the example below, I set the onDemand attribute to false by adding the  &#8216;onDemand=&#8221;false&#8221;&#8216; tag and the control did, in fact, get instantiated at load time, giving me the ability to disable it.

<pre>&lt;Tool id="MGS_QDIAddin_QDIAddTool" class="QDIAddTool" message="QDI Add New Location." caption="QDI Add Tool" tip="QDI Add Tool." category="QDI Add-In Controls" image="ImagesQDIAddTool.png" onDemand="false"/&gt;
</pre>