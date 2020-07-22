---
id: 675
title: 'ArcMap Field Calculator: Text to Double'
date: 2012-03-28T05:23:32-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=675
permalink: /2012/03/28/arcmap-field-calculator-text-to-double/
twitter_cards_summary_img_size:
  - 'a:6:{i:0;i:651;i:1;i:603;i:2;i:3;i:3;s:24:"width="651" height="603"";s:4:"bits";i:8;s:4:"mime";s:9:"image/png";}'
categories:
  - ArcGIS 10
  - ArcMap
  - arcpy
  - Field Calculator
  - Python
---
Received a request yesterday asking how to use the ArcMap Calculator to copy values from a Text field to a Double field using python syntax.&nbsp; As any good blogger would do, I immediately thought, &#8220;Awesome! Someone&#8217;s question is the perfect topic for a new blog post&#8221;.

The python parser is actually pretty good at casting values on the fly so if the values in your text field (!Day! in my example) are valid values that can be converted to a Double value, it is as simple as just setting the formula to be the text field. In my example case, I wanted to copy the value from !Day! to !DecDay! so I set the formula to be DecDay = !Day!.

[<img class="aligncenter size-full wp-image-676" title="ToNumber1" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2012/03/tonumber1.png?resize=614%2C568" alt="" width="614" height="568" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2012/03/tonumber1.png)

That should work fine if you have clean values in your text field. In the example above, you might notice I had a selected set of 3 records that all had numeric values in the !Day! field. When I included the fourth row, which does not have a numeric value in the text field, I get this error message (&#8220;There was a failure during processing, check the Geoprocessing Results window for details.&#8221; when I use the same formula. Time to add in an error exception.

[<img class="aligncenter size-full wp-image-682" title="ToNumber2b" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2012/03/tonumber2b.png?resize=516%2C191" alt="" width="516" height="191" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2012/03/tonumber2b.png)

For more advanced logic, the Field Calculator dialog allows you to use a python function if you check on the &#8220;Show Codeblock&#8221; option.&nbsp; In the &#8220;Pre-Logic Script Code&#8221; area (Seriously, who at ESRI came up with that name?) I entered the following function. If the value in my text field (!Day!) can be cast to a number of type float, that value is returned. If the cast is unsuccessful (IE the value in !Day! is not a number), then I return -99.

<pre>def toNum(inValue):
   try:
      outValue = float(inValue)
      return outValue
   except:
      return -99</pre>

Then in the formula portion of the dialog, I call the function, passing the value in the !Day! field: DecDay = toNum(!Day!).  
[<img class="aligncenter size-full wp-image-677" title="ToNumber2" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2012/03/tonumber2.png?resize=614%2C574" alt="" width="614" height="574" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2012/03/tonumber2.png)

Now, if you would prefer not to set all the records with non-numeric values to be -99 or other error value, not return anything. To do this, I replaced the &#8220;return -99&#8221; in the original function with a filler line (&#8220;doNothing = 4&#8221;) since the try block needs an non-empty except clause.

<pre>def toNum(inValue):
   try:
      outValue = float(inValue)
      return outValue
   except:
      doNothing = 4
</pre>

[<img class="aligncenter size-full wp-image-683" title="ToNumber3" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2012/03/tonumber3.png?resize=485%2C180" alt="" width="485" height="180" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2012/03/tonumber3.png)  
And that should leave the values in the double field unscathed in your records with non-numeric values in the text field.

Shameless Plug: Check out my other blog posts on using ArcMap&#8217;s Field Calculator to [calculate geometry](http://maprantala.com/2011/03/08/calculating-geometry-using-arcpy-in-field-calculator/) and converting a date value to an [8 digit numeric value](http://maprantala.com/2011/10/06/field-calculator-arcpy-date-to-decimal-function/).