---
id: 687
title: 'ArcMap Field Calculator: Beware of Integer Division!'
date: 2012-04-02T05:34:54-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=687
permalink: /2012/04/02/arcmap-field-calculator-beware-of-integer-division/
categories:
  - ArcGIS 10
  - ArcMap
  - Field Calculator
---
Apparently, if you post one time about ArcMap field calculator, you&#8217;re bound to get additional questions.  After [my recent post](http://maprantala.com/2012/03/28/arcmap-field-calculator-text-to-double/) about using field calculator to convert text values to numeric, someone asked about a problem they were having with another calculation they were having.

The underlying problem was that python 2.6, which is installed with ArcGIS 10, [uses integer division](http://docs.python.org/release/2.6.7/library/stdtypes.html#numeric-types-int-float-long-complex) when both the numerator and denominator are integers. The result of integer division is an integer rounded towards negative infinity.

If you&#8217;re not aware of this (or forget) then you open yourself up to some unexpected results&#8211;they can be especially hard to catch if you are using it within a larger block of code.

In this example, I&#8217;m calculating a percentage but the result is 0 for all the records because of the rounding.

[<img class="aligncenter size-full wp-image-688" title="Calc1" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2012/03/calc1.png?resize=614%2C432" alt="" width="614" height="432" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2012/03/calc1.png)

The easiest thing to do in this simple example is just convert one of the two value to a non-integer value. This can be done by multiplying by 1.0 (not just &#8220;1&#8221;, you need to include the &#8220;.0&#8221;) which is a float datatype. Multiplying one of the values by a float makes that value a float and integer division no longer applies & we end up with a happy GISer.

[<img class="aligncenter size-full wp-image-689" title="Calc2" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2012/03/calc2.png?resize=614%2C431" alt="" width="614" height="431" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2012/03/calc2.png)

Another option if you are using a Codeblock is to include &#8220;from \_\_future\_\_ import division&#8221; in your code block. Python is slowly moving away from using integer division and the \_\\_\_future\_\__ module overrides the default behavior.

[<img class="aligncenter size-full wp-image-690" title="Calc3" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2012/03/calc3.png?resize=614%2C430" alt="" width="614" height="430" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2012/03/calc3.png)