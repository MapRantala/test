---
id: 731
title: 'Quick &#038; Dirty python: Converting a text file to audio (.wav)'
date: 2012-05-31T23:12:28-05:00
author: Matt
excerpt: 'A quick &amp; dirty python snippet that converts a text file to audio using the Microsoft Speech API (SAPI). Now you can listen to those boring documents!'
layout: post
guid: http://maprantala.com/?p=731
permalink: /2012/05/31/quick-dirty-python-converting-a-text-file-to-audio-wav/
categories:
  - Python
  - 'Quick &amp; Dirty'
tags:
  - arcpy
  - Microsoft Speech API
  - pyTTS
  - sapi
  - text to audio
  - text to mp3
  - text to speech
  - text to wav
---
This is a bit of a tangent but for some crazy reason, I wanted to convert some text to audio so I could listen to it while I drive. A quick Google search left me without any freeware that could handle the 53 page document&#8211;there are some cool websites that do text to mp3 like <a href="http://vozme.com/index.php?lang=en" target="_blank">vozme </a>and <a href="http://www.yakitome.com/" target="_blank">YAKiToMe</a>! but they didn&#8217;t convert the whole document. I then found <a href="http://www.cs.unc.edu/Research/assist/doc/pytts/" target="_blank">pyTTS</a>, a python package that serves as a wrapper to the <a href="http://msdn.microsoft.com/en-us/library/ms723627%28v=vs.85%29" target="_blank">Microsoft Speech API (SAPI) </a>, which has <a href="http://en.wikipedia.org/wiki/Microsoft_Speech_API" target="_blank">been in version 5 since 2000.</a> But I didn&#8217;t easily find a version of pyTTS for python 2.6. So I decided to see if I could roll my own.

As it turns out, getting python to talk using SAPI is relatively easy. Reading a plain text file can be done in a few lines.

<pre>from comtypes.client import CreateObject

infile = "c:/temp/text.txt"

engine = CreateObject("SAPI.SpVoice")

f = open(infile, 'r')
theText = f.read()
f.close()

engine.speak(theText)</pre>

And it wasn&#8217;t that much more to have it write out a .wav file:

<pre>from comtypes.client import CreateObject

engine = CreateObject("SAPI.SpVoice")
stream = CreateObject("SAPI.SpFileStream")

infile = "c:/temp/text.txt"
outfile = "c:/temp/text4.wav"
stream.Open(outfile, SpeechLib.SSFMCreateForWrite)
engine.AudioOutputStream = stream

f = open(infile, 'r')
theText = f.read()
f.close()

engine.speak(theText)

stream.Close()</pre>

And with that chunk of code, I was able to convert my 54 page document into a 4 hour long .wav file (over 600 MB) that I used another software package to convert to .mp3 (200 MB). The voice is a bit robotic but not too bad, I just hope the content that I converted (a database specification standard) doesn&#8217;t put me to sleep while I drive.