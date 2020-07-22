---
id: 87
title: Entering a Null Value in VB.Net DateTimePicker
date: 2010-07-13T16:26:45-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=87
permalink: /2010/07/13/datetimepicker-null-value/
geo_latitude:
  - "0"
geo_longitude:
  - "0"
geo_accuracy:
  - "0"
geo_public:
  - "1"
categories:
  - VB.Net
tags:
  - date
  - datetimepicker
  - handles
  - keyeventargs
  - null value
  - query
  - vb.net
---
While working on a VB.Net form to query a database that includes an optional date range query, I decided I wanted to display a datetimepicker with a blank value by default and then, if the user sets a date show the date.  By default, a datetimepicker can not have a Null date, so I went a&#8217;Googling and found this post, [.NET Reference: Setting the DateTimePicker to a Blank Value](http://dotnetref.blogspot.com/2009/03/setting-datetimepicker-to-blank-value.html) ,that had exactly the technique I wanted.

Basically, I set the datetimepicker&#8217;s CustomFormat to a space (&#8221; &#8220;) when I want it to be Null and then when the user makes a selection, set the custom format to the date format I wanted to display.    This is the sample code I found:

<pre>Private Sub ToDateField_ValueChanged(ByVal sender As System.Object, ByVal e As System.EventArgs) _
   Handles ToDateField.ValueChanged
     Me.ToDateField.Format = Windows.Forms.DateTimePickerFormat.Custom
     Me.ToDateField.CustomFormat = "M/dd/yyyy"
End Sub
</pre>

To let the user delete the date they entered, I add this subroutine that handles the KeyDown event for the datetimepicker. If the user hits delete or backspace, I set the format back to my null CustomFormat.

<pre>Private Sub ClearDates_KeyPress(ByVal sender As Object, ByVal e As System.Windows.Forms.KeyEventArgs) _
 Handles ToDateField.KeyDown

 If (e.KeyCode = Windows.Forms.Keys.Delete) Or (e.KeyCode = Windows.Forms.Keys.Back) Then
 Dim mDateTimePicker As Windows.Forms.DateTimePicker
          mDateTimePicker = TryCast(sender, Windows.Forms.DateTimePicker)
          If Not (mDateTimePicker Is Nothing) Then
               mDateTimePicker.CustomFormat = " "
          End If
End If
End Sub
</pre>

When building my SQL statement, I check the .CustomFormat property to determine whether or not to include a condition for the data:

<pre>If Not (Me.ToDateField.CustomFormat = " ") Then
 'Code here to add to my SQL statement
 End If
</pre>

<div id="geo-post-87" class="geo geo-post" style="display: none">
  <span class="latitude"></span><span class="longitude"></span>
</div>