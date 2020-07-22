---
id: 263
title: Displaying Node Dangles in Arcedit
date: 2011-01-06T16:50:42-06:00
author: Matt
layout: post
guid: http://maprantala.com/?p=263
permalink: /2011/01/06/displaying-node-dangles-in-arcedit/
categories:
  - ESRI
  - Workstation Arc/Info
tags:
  - arcedit
  - ESRI
  - node dangles
  - Workstation Arc/Info
---
Since the name of the blog is Node Dangles, I get several hits daily from searches on &#8220;Node Dangles&#8221; and I have no  information on node dangles.  This post is the first in a series to change that.

First let us, by us, I mean [ESRI](http://www.esri.com), define what a node dangle is.  Their online glossary actually defines a [dangling arc](http://resources.arcgis.com/glossary/term/215), a dangling node is a [node](http://resources.arcgis.com/glossary/term/71) (endpoint of an arc) that does the non-connecting mentioned below:

<p style="padding-left:30px;">
  &#8220;An arc having the same polygon on both its left and right sides and having at least one node that does not connect to any other arc. It often occurs where a polygon does not close properly, where arcs do not connect properly (an undershoot), or where an arc was digitized past its intersection with another arc (an overshoot). A dangling arc is not always an error; for example, it can represent a cul-de-sac in a street network.&#8221;
</p>

Matt&#8217;s definition is &#8220;a node that isn&#8217;t connected to any other node&#8221;.  The Arcedit screen shot below shows a coverage of roads and its arcs (white lines), nodes (white squares) at the ends of the arcs, and node dangles (red squares) indicating what nodes are dangling.  Too bad clearly flagging [dangling chads](http://en.wikipedia.org/wiki/United_States_presidential_election,_2000) isn&#8217;t so eary .

[<img class="aligncenter size-full wp-image-264" title="Node Dangles in ArcEdit" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2011/01/dangles.jpg?resize=629%2C598" alt="" width="629" height="598" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2011/01/dangles.jpg)

Displaying (and illustrating) how to display dangling nodes in Arcedit is simple, the command is &#8220;drawenvironment node dangle&#8221; or, as I&#8217;ve done above, &#8220;drawe node dangle&#8221;.  To display the dangles in a different color than the default white, follow-up with the command &#8220;nodecolor dangle 2&#8221;.  The 2 is ESRI&#8217;s value for red.