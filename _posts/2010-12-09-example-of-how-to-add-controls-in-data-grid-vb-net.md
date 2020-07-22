---
id: 231
title: Example of how to add controls in data grid VB.NET
date: 2010-12-09T17:15:35-06:00
author: Matt
layout: post
guid: http://maprantala.com/?p=231
permalink: /2010/12/09/example-of-how-to-add-controls-in-data-grid-vb-net/
categories:
  - ArcObjects
  - VB.Net
tags:
  - ArcObjects
  - combobox
  - datagrid
  - vb.net
---
I have been working on some data entry forms that utilize a DataGrid.  Using a PostGres Geodatabase that had domains set on several fields, I could not directly bind to the controls on my dialog.  So I am going the round-about way of populating my own comboboxes with valid names and displaying within the DataGrid.

Having not done this previously, I found this example: [Example of how to add controls in data grid VB.NET](http://www.eggheadcafe.com/community/aspnet/14/22477/example-of-how-to-add-controls-in-data-grid.aspx) very useful and just wanted to point it out to anyone.  [George Shephard&#8217;s Windows Forms FAQ](http://www.syncfusion.com/FAQ/windowsforms/faq_c44c.aspx#q1015q) also had several useful tips.

Finally, I has a problem with the masked textboxs I added to the Datagrid, they required users to click once to get focus and a second to start editing.  After much googling, I found used some information from [MSDN](http://social.msdn.microsoft.com/Forums/en-US/vbgeneral/thread/8d3c1f98-9501-4902-b81b-82f6bbd4f3e5/) that allowed me to find a work-around.  In my Mousedown event, I included this snippet (QdiControl is the control for the specific cell that the HitTestInfo says the mousedown hit):

<pre>&lt;/pre&gt;
Dim WindowsControl As System.Windows.Forms.Control = CType(QdiControl, System.Windows.Forms.Control)
If (TypeOf WindowsControl Is MaskedTextBox) Then
 Dim pTextBox As MaskedTextBox = CType(WindowsControl, MaskedTextBox)
 Dim dgdtblStyle As DataGridColumnStyle = RelatedForm.DataGrid.TableStyles(0).GridColumnStyles(CurrentColumn)

 RelatedForm.DataGrid.BeginEdit(dgdtblStyle, CurrentRow)
 pTextBox.Select(pTextBox.Text.Length, 0)
 WindowsControl.Focus()

 End If
</pre>

Peace.