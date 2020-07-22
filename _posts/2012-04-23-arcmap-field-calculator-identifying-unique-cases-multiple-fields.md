---
id: 705
title: 'ArcMap Field Calculator: Identifying Unique Cases, Multiple Fields'
date: 2012-04-23T06:06:25-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=705
permalink: /2012/04/23/arcmap-field-calculator-identifying-unique-cases-multiple-fields/
categories:
  - ArcGIS
  - ArcMap
  - arcpy
  - ESRI
  - Field Calculator
  - Python
tags:
  - arcgis
  - ArcMap
  - arcpy
  - Field Calculator
---
You may have noticed that this post&#8211;[ArcMap Field Calculator: Identifying Unique Cases, Single Field](http://wp.me/pVrsJ-bf)&#8211;specifies &#8220;Single Field&#8221;. Yes, that was my version of a cliff-hanger post.

The basic structure I listed in that post can be expanded on to satisfy your needs. The example in my earlier post was case sensitive for example, you could modify it so it treats &#8220;a&#8221; the same as &#8220;A&#8221;.

Today&#8217;s example groups records into different cases based off the values of two fields, !county_c! and !feature! and required only minor modifications.

The calling line was modified from:

<p style="padding-left:30px;">
  returnCase(!feature!)
</p>

to:

<p style="padding-left:30px;">
  returnCase(!county_c!,!feature!)
</p>

to accommodate passing both values.

The function definition likewise was modified to accept two values, this:

<p style="padding-left:30px;">
  def returnCase(inValue1):
</p>

to:

<p style="padding-left:30px;">
  def returnCase(inValue1, inValue2)
</p>

And this line was added, creating a list from the two values passed in:

<p style="padding-left:30px;">
  inValue = [inValue1, inValue2]
</p>

&nbsp;

**(Note: The same results could have be achieved by using the original function by creating the list in the calling statement:  returnCase([!county_c!,!feature!] )  
** 

&nbsp;

<pre>caseList = [ ]

def returnCase(inValue1, inValue2):
   inValue = [inValue1, inValue2]
   global caseList

   if not inValue in caseList:
      caseList.append(inValue)

   return caseList.index(inValue)</pre>

[<img class="aligncenter size-full wp-image-707" title="Case_Multiple" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2012/04/case_multiple.png?resize=614%2C462" alt="" width="614" height="462" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2012/04/case_multiple.png)