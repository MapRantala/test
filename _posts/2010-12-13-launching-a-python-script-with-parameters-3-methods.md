---
id: 252
title: 'Launching a Python script with parameters&#8211;3 methods.'
date: 2010-12-13T14:56:43-06:00
author: Matt
layout: post
guid: http://maprantala.com/?p=252
permalink: /2010/12/13/launching-a-python-script-with-parameters-3-methods/
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
  - ArcGIS
  - ArcObjects
  - Model Builder
  - Python
tags:
  - arcgis
  - arctoolbox
  - python
  - python parameters
  - tkinter
---
Since I use python for different tasks, I launch python scripts a variety of ways. Depending on what I am doing, a single script may need to accept parameters from either:

  1. Passed in from an ArcGIS Toolbox Tool.
  2. Re-occurring default value.  Often used in scheduled processes, a nightly backup, for example.
  3. A temporary set of values used in an interactive, debugging session.

What I often do is make the parameter interpretation flexible to meet my needs.  The sample below shows how I do this.  The logic first checks to see if the correct number of parameters were used to launch the script (i.e. if it is called by an Arc Toolbox tool), where the files for the default files exist or if the debug values are valid, including checking the current date against a hard-coded date variable.  Juggling the conditional structure would allow you to prioritize the options differently.

I am also using Tkinter to display an interactive dialog if none of the three conditions are successfully met.

<pre>import os.path
import datetime
import shutil
import sys

theEmailText = "nStart: " + str(datetime.datetime.now())

#This example shows three different ways for a script to receive input paramters.
#
# First, if it was run with the necessary number of parameters, as if launched by
# an ArcToolbox tool, it uses those parameters.

if len(sys.argv) == 2:
    wellsShapeFile = sys.argv[0]
    unwellsShapeFile = sys.argv[1]
    theEmailText = theEmailText + "nUsing Parameters Passed In:nn Located Wells File: "+wellsShapeFile+"n Unlocated Wells File: "+unwellsShapeFile
else:
    dateString = datetime.date.today().strftime("%Y%m%d")
    wellsShapeFile = "C:/cwi5_bk/wells/temp/wells_.shp"
    unwellsShapeFile = "C:/cwi5_bk/wells/temp/unloc_wells.shp"

    #Second attempt is if there are default values that should be used.
    #I use this for a process that is run via scheduled Windows Task
    if (os.path.exists(wellsShapeFile)) and (os.path.exists(unwellsShapeFile)):
        theEmailText = theEmailText + "nUsing Automated Date-Based file names:nn Located Wells File: "+wellsShapeFile+"n Unlocated Wells File: "+unwellsShapeFile
    else:
        #The third method is used for debugging/running in the IDE.
        #I put a check condition so this is valid for one day only
        #And then hard-code the temporary paths.
        #
        #Note that you may want to modify the structure of the IF statement
        #used for methods 2 & 3 so that it checks for the manual-override (3rd)
        #method first
        if dateString == '20101213':
            wellsShapeFile = "C:/cwi5_bk/wells/temp/wells.shp"
            unwellsShapeFile = "C:/cwi5_bk/wells/temp/unloc.shp"
            theEmailText = theEmailText + "nUsing Manual Override file names:nn Located Wells File: "+wellsShapeFile+"n Unlocated Wells File: "+unwellsShapeFile
        else:
            theEmailText = theEmailText + "n Manual Override for file names does not meet data filter"

    if (not os.path.exists(wellsShapeFile)) or (not os.path.exists(unwellsShapeFile)):
        from Tkinter import *
        msgbox = Tk()
        msgbox.title('Error')
        Message(msgbox,text="Must either use ArcTool to launch or edit file parameters", bg='royalblue',fg='ivory', relief=GROOVE).pack(padx=10, pady=10)
        msgbox.mainloop()
        print theEmailText
        quit()

print theEmailText
</pre>

<div id="geo-post-252" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>