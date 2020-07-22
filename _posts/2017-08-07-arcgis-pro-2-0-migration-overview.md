---
id: 4706
title: ArcGIS Pro 2.0 Migration Overview
date: 2017-08-07T19:48:08-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=4706
permalink: /2017/08/07/arcgis-pro-2-0-migration-overview/
categories:
  - ArcGIS Pro 2.0
  - Career
  - Misc
tags:
  - ArcGIS Pro
  - Software
---
In April I started a new position at a company that had no existing GIS.

Nothing.

There was a definite need for GIS and some GIS-type functions were occuring but basically when I started, I had an <a href="http://server.arcgis.com/en/server/latest/get-started/windows/what-is-arcgis-enterprise-.htm" target="_blank" rel="noopener">ArcGIS Enterprise</a> license and a mess of KML files.

An exciting opportunity. And since I was starting from scratch, I had zero legacy concerns. No existing data, workflows, custom code, or maps to tie me to a specific software package. Knowing that ArcGIS Pro is taking over the world, I decided to transition to ArcPro and use that as our (GIS staff count of 1) company standard for desktop mapping.

That plan got side-tracked somewhat when we purchased a data management tool (<a href="http://crescentlink.com/products#manager" target="_blank" rel="noopener">CrescentLink Network Manager</a>) that is only available for ArcMap. But I have still been using Pro on a regular basis.

I started a brilliant post about the transition&#8211;the pros, the cons, and my overall experience. But before I could finish it, <a href="https://blogs.esri.com/esri/arcgis/2017/06/27/arcgis-pro-2-0-has-been-released/" target="_blank" rel="noopener">out came 2.0</a> and my post was suddenly out-dated. So I&#8217;ve decided to split that post into a bunch of smaller, more focused posts.

#### Overview

Without getting into to deep on specific features or functionality, I do have some broad comments to make.

###### Learning Curve

There is definitely a learning curve in moving from ArcGIS Desktop to ArcGIS Pro&#8211;even though there is a lot of the same concepts between Pro & Desktop (the Toolboxes have barely changed), just finding the tools was a huge hurdle at first. I&#8217;ve never been a fan of the ribbon interface, I like my tools to be where they are and accessible.

But with regular use, I have gotten more adept at finding what I want to use. Still end up hunting for tools at times but it has gotten better. I&#8217;m probably at about 75% efficiency as compared to Desktop although I switch between the two on a regular basis because there are times where I just need to get something done.

###### Stability

Even though I just had to force-stop a session, 2.0 has made significant strides in stability. I had near daily crashes with 1.5 but now it is maybe a once-a-week. Maybe I&#8217;ve learned what not to do in Pro but I don&#8217;t remember any pattern to the crashes I had with 1.5.

###### Performance

Performance is still painful to me. There are too many times when the wait cursor shows up when you do simple things like clicking on a button. A lot of the processing has been routed through the geoprocessing system and it just seems much slower.

#### Version 2.0 Highlights

While I&#8217;ll probably do some whining about ArcGIS Pro in this planned series of posts, there were a couple of significant highlights to the 2.0 release that took care of two of my major usability concerns.

<li style="list-style-type: none;">
  <ul>
    <li>
      Simultaneously running multiple instances of ArcGIS on the same machine. I did some initial scripting for a data preparation process. The script took a good 15-20 minutes to run. Using 1.5, I had to launch it and then work in something other than ArcGIS Pro. Now I can launch that process and continue to work in a second session.
    </li>
    <li>
      Highlighting. Maybe I am just weird but I really missed the ability to highlight records in table. As part of my QC process, I will often select records using a spatial or attribute query and then go through that set and either unselect or reselect them in batches by first highlighting them. This was a huge issue I had with 1.5 especially since it does not seem like it should be difficult functionality to add in. I was going to wait on 2.0 until I saw this functionality.<a href="https://i2.wp.com/maprantala.com/wp-content/uploads/2017/08/Highlight-1.png"><img class="aligncenter wp-image-4715 " src="https://i2.wp.com/maprantala.com/wp-content/uploads/2017/08/Highlight-1.png?resize=886%2C374" alt="" width="886" height="374" data-recalc-dims="1" /></a>
    </li>
  </ul>
</li>

#### Summary

Overall, I&#8217;ve grown accustom to ArcPro. Like any new software, it takes time to get a feel for it. The transition is not that much different from going from ArcView 3.x to ArcGIS Desktop (or whatever 8.x was called). I am not yet as productive in it as Desktop but there are some things I really like about it to go along with my complaints.