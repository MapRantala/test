---
id: 806
title: 'ArcMap Field Calculator: Calculating Running Total with arcpy'
date: 2014-02-13T06:06:37-06:00
author: Matt
layout: post
guid: http://maprantala.com/?p=806
permalink: /2014/02/13/arcmap-field-calculator-calculating-running-total-with-arcpy/
publicize_twitter_user:
  - MapRantala
publicize_twitter_url:
  - http://t.co/6A78IUCZhg
publicize_linkedin_url:
  - 'http://www.linkedin.com/updates?discuss=&scope=24792516&stype=M&topic=5839701277625315328&type=U&a=EtSo'
categories:
  - Misc
---
I had a user with a series of GPS points (that were in chronological order) that they wanted to know the accumulated distance from the start to each point in their shapefile. First, we calculated the distance from each point to the previous point into a field called [DistFt].

Then, we hacked out this quick python function to accumulate the total distance in Arcmap&#8217;s Field Calculator:

<pre>totalDistance = 0

def accumulateDistance(inDist):
   global totalDistance
   totalDistance += inDist
   return totalDistance</pre>

And we called it:

<pre>accumulateDistance(!DistFt!)</pre>

[<img class="aligncenter size-full wp-image-809" alt="accumulateDist" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2014/02/accumulatedist.png?resize=508%2C604" width="508" height="604" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2014/02/accumulatedist.png)And we got what we wanted.