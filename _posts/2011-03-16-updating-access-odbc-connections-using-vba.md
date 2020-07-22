---
id: 456
title: Updating Access ODBC connections using VBA
date: 2011-03-16T20:06:59-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=456
permalink: /2011/03/16/updating-access-odbc-connections-using-vba/
geo_latitude:
  - "44.852994"
geo_longitude:
  - "-93.55073"
geo_accuracy:
  - "1447"
geo_address:
  - 8341-8345 Suffolk Dr, Chanhassen, MN 55317, USA
geo_public:
  - "1"
categories:
  - VBA
tags:
  - access
  - odbc
  - odbc connection
  - pass-through query
  - query
  - vba
---
I don&#8217;t do much in Access or VBA but sometimes you inherit things that force you, at least temporarily, to go retro.

We have an Access-based data extraction routine that uses several (21) pass-through queries that use an ODBC connection.  The queries are pulling data from an Oracle database maintained by a different agency that enforces a time-out of passwords every three months.

So every three months I have to go through the ODBC connections for these queries and change the password.  And before you chastise me for storing the password in the VBA code, I can only say &#8220;you&#8217;re right&#8221; but that&#8217;s the way it is done&#8211;not ideal but gets the job done and we&#8217;re not dealing with overly sensitive data.

Well, my three-month deadline was approaching and I figured I would simplify the process which I found error-prone.

Some searching and object model diagramming scouring and I came up with this barebones solution, of course, your connection string may vary.

<pre>Public Sub UpdateConnectStrings()

Dim obj As AccessObject
Dim dbs As Object
Set dbs = Application.CurrentData

Dim i As Integer

Dim newConnectionString As String
newConnectionString = "ODBC;DRIVER={Microsoft ODBC for Oracle};SERVER=vader;UID=luke;PWD=iMufathr;"

Dim comparisionConnectionString As String
comparisionConnectionString = "ODBC;Driver"

For Each obj In dbs.AllQueries

If (obj.Type = acQuery) Then
  For i = 0 To CurrentDb.QueryDefs.Count - 1
    If (CurrentDb.QueryDefs(i).Name = obj.Name) Then
      If (Left(CurrentDb.QueryDefs(i).Connect, Len(comparisionConnectionString)) = comparisionConnectionString) Then
        CurrentDb.QueryDefs(i).Connect = newConnectionString
      End If
    End If
  Next i
End If

Next obj
End Sub

</pre>

<div id="geo-post-456" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>