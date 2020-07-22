---
id: 67
title: Python emailer
date: 2010-07-09T17:20:38-05:00
author: Matt
excerpt: Use python to send eMails from a Google account.
layout: post
guid: http://maprantala.com/?p=67
permalink: /2010/07/09/python-emailer/
categories:
  - Python
tags:
  - code
  - email
  - python
---
So I have a nightly process that runs and I, being the lazy programmer I am, didn&#8217;t want to bother checking a log file each morning to see how it went. The natural answer is to have the results emailed to me because I do have to check my email.

Since the process is already handled mostly in python, thought I would implement the emailing via python. It was actually simple enough to do&#8211;just required figuring out the setting for my SMTP server (actually I didn&#8217;t figure it our for my work server so I&#8217;m using my GMail account).

I first wrote eMailer.py (see code below) and included it in my python path. Now, whenever I want to send code from a python application, it takes two easy lines:

<pre>import eMailer
eMailer.SendMess("someoneg@company.com","Subject","Text Body")
</pre>

## eMailer.py

<pre>import smtplib
from email.MIMEMultipart import MIMEMultipart
from email.MIMEText import MIMEText

fromaddr = 'node.dangles AT gmail.com'
password = 'myFancyPassword'
smtpaddr = 'smtp.gmail.com'
smtpport = 587

def SendMess(toadd, subject, body):

msg = MIMEMultipart()
msg['From'] = fromaddr
msg['To'] = toadd
msg['Subject'] = subject
msg.attach(MIMEText(body, 'plain'))

text = msg.as_string()

server = smtplib.SMTP(smtpaddr, smtpport)
server.ehlo()
server.starttls()
server.ehlo()
server.login(fromaddr, password)

server.sendmail(fromaddr, toadd, text)
</pre>