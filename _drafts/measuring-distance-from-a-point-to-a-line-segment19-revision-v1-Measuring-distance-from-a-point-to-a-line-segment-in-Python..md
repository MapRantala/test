---
id: 4856
title: Measuring distance from a point to a line segment in Python.
date: 2019-01-20T09:58:39-06:00
author: adminguy
excerpt: Python implementation of calculating distance form a point to a line segment.
layout: revision
guid: https://maprantala.com/2019/01/20/19-revision-v1/
permalink: /2019/01/20/19-revision-v1/
---
I recently had the need to calculate the distance from a point (address point) to a polyline (street segment) and wanted to avoid using any additional libraries because it was being done for an external client. Ok, I actually used arcgisscripting for reading the data but that lacked, from what I could tell, the fine-detail granularity of measuring distance between individual geometries.

But since the only spatial operations I needed were to measure the distance between two points and the distance between a point and polyline, I decided to just do it via brute force. The datasets being processed were not huge so it made this feasible.

Measuring point-to-point distance was easy enough. And while I knew I could work through the details of measuring the distance between a point and polyline, I decided NOT to re-invent the wheel if I did not have to. Google did not come up with any direct answers but I did come across this [post](http://local.wasp.uwa.edu.au/%7Epbourke/geometry/pointline/) from [Paul Bourke](http://local.wasp.uwa.edu.au/%7Epbourke/ "Paul Bourke") at the [University of Western Australia](http://wasp.uwa.edu.au/) about measuring the distance between a point and a line segment. A polyline is just a series of line segments so I used it as the basis of my measurement&#8211;I just loop through all the consecutive vertice pairs, using the minimum distance found.

Paul did not have a python implementation included, so I went ahead and created my own from his information.

<pre>def lineMagnitude (x1, y1, x2, y2):
    lineMagnitude = math.sqrt(math.pow((x2 - x1), 2)+ math.pow((y2 - y1), 2))
    return lineMagnitude

#Calc minimum distance from a point and a line segment (i.e. consecutive vertices in a polyline).
def DistancePointLine (px, py, x1, y1, x2, y2):
    #http://local.wasp.uwa.edu.au/~pbourke/geometry/pointline/source.vba
    LineMag = lineMagnitude(x1, y1, x2, y2)

    if LineMag &lt; 0.00000001:
        DistancePointLine = 9999
        return DistancePointLine

    u1 = (((px - x1) * (x2 - x1)) + ((py - y1) * (y2 - y1)))
    u = u1 / (LineMag * LineMag)

    if (u &lt; 0.00001) or (u &gt; 1):
        #// closest point does not fall within the line segment, take the shorter distance
        #// to an endpoint
        ix = lineMagnitude(px, py, x1, y1)
        iy = lineMagnitude(px, py, x2, y2)
        if ix &gt; iy:
            DistancePointLine = iy
        else:
            DistancePointLine = ix
    else:
        # Intersecting point is on the line, use the formula
        ix = x1 + u * (x2 - x1)
        iy = y1 + u * (y2 - y1)
        DistancePointLine = lineMagnitude(px, py, ix, iy)

    return DistancePointLine</pre>