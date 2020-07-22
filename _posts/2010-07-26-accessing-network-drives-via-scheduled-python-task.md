---
id: 152
title: Accessing Network Drives via Scheduled Python Task
date: 2010-07-26T12:43:02-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=152
permalink: /2010/07/26/accessing-network-drives-via-scheduled-python-task/
categories:
  - Python
tags:
  - .bat
  - backups
  - batch files
  - network drives
  - python
  - task scheduler
---
As previously mentioned, I have a scheduled nightly backup that is written in Python.&nbsp; Most of it has been working fine but I had not gotten it to copy files to network drives.&nbsp; I finally got around to correcting that part.&nbsp; My first attempt was just to map the network drives for the user account that is used to run the task.&nbsp; No good.

My second attempt was to call a .bat file from python before accessing the network drives and that worked.

The batch file has two lines (one for each network drive):

net use n: \domain1sharename mypassword /USER:domainusername  
net use t: \domain2sharename mypassword /USER:domainusername

And my python script includes two lines to call it before I access the network locations:

<pre>import os
os.system('C:/cwi5_bk/mapDrive.bat')
</pre>