---
id: 583
title: Debugging a Python Scheduled Task
date: 2011-07-11T18:11:13-05:00
author: Matt
layout: post
guid: http://maprantala.com/?p=583
permalink: /2011/07/11/debugging-a-python-scheduled-task/
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
  - arcpy
  - Bugs
  - Python
tags:
  - arcgis
  - arcpy
  - python
  - Windows Task Scheduler
---
I have been working on a python script that I want (NEED) to run as a scheduled task on a remote machine.  I got to the point that the script did exactly what I needed when I was interactively running it in a Windows session but had problems when running it as a scheduled task.  The debugging process was cumbersome&#8211;make a change, schedule a task to run it, log out of the machine, and wait.  The log back in and repeat the process.

That got old.

So I wrote [a script  (tester.py)](http://dl.dropbox.com/u/22241283/NodeDangles/20110711_tester.zip) that calls any other python scripts in the same directory that (1) start with &#8220;test\_&#8221; and (2) there is not a corresponding file with the same base name and &#8220;.start&#8221; extension.  It would launch &#8220;test\_BaBing.py&#8221; as long as there is not a &#8220;test_BaBing.start&#8221; in the same directory.  Tester.py continued to run, looping every 60 seconds, until tester.stop exists.

This made the process easier because I could work on my local machine, editing the problematic script, saving changes and within 60 seconds it would be launched on the remote machine.  I could view the results, make additional edits, delete the .start file and it would launch again within 60 seconds.

Within a couple minutes I was able to determine the problem (path related) and fix it.

Happy programmer.

<disclaimer>I would recommend using this only while debugging a script&#8211;routinely running it could be a security risk since someone could copy a destructive python script into the directory and this would run it.</disclaimer>

Download: [tester.py](http://dl.dropbox.com/u/22241283/NodeDangles/20110711_tester.zip)

<pre>import sys, string, os
import glob
import datetime, shutil
import time, inspect
import getpass

totalstarttime = datetime.datetime.now()

dateString = datetime.date.today().strftime("%Y%m%d_")+datetime.datetime.now().strftime("%H%M%S") #datetime.date.today().strftime("%Y%m%d")
debugfile = inspect.getfile(inspect.currentframe()).replace(".py","_"+dateString+"_Debug.txt")
stopfile = inspect.getfile(inspect.currentframe()).replace(".py",".stop")
newdebugfile = False

codeDir = os.path.dirname(inspect.getfile(inspect.currentframe())).replace("\","/")

def printit(inText):
    global newdebugfile

    print inText

    if os.path.exists(debugfile):
        if (newdebugfile == False):
            tmpfile = open(debugfile,"w")
            newdebugfile = True
        else:
            tmpfile = open(debugfile,"a")
    else:
        tmpfile = open(debugfile,"w")

    tmpfile.write(inText)
    tmpfile.write("n")
    tmpfile.close()
    newdebugfile = True

stopFileExists = False
printit("Code Directory: "+codeDir)
printit("Starting at: "+datetime.date.today().strftime("%Y-%m-%d_")+datetime.datetime.now().strftime("%H:%M:%S"))
printit("Stopfile : "+stopfile+"/n")
while (stopFileExists == False):
    for iFile in glob.glob(codeDir+"/test_*.py"):

        thisStartfile = iFile.replace(".py",".start")

        if not (os.path.exists(thisStartfile)):
            printit ("Launching: "+iFile)
            iTmpfile = open(thisStartfile,"w")
            iTmpfile.write("started")
            iTmpfile.close()
            os.system("Start "+iFile)

    if (os.path.exists(stopfile)):
        stopFileExists = True
    else:
        time.sleep(60)

    printit("nEnd of Loop: "+datetime.date.today().strftime("%Y-%m-%d_")+datetime.datetime.now().strftime("%H:%M:%S")+"n")    

printit("Done!")</pre>

<div id="geo-post-583" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>