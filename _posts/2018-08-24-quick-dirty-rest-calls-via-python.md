---
id: 4795
title: 'Quick &#038; Dirty: REST calls via python'
date: 2018-08-24T18:11:30-05:00
author: Matt
layout: post
guid: https://maprantala.com/?p=4795
permalink: /2018/08/24/quick-dirty-rest-calls-via-python/
categories:
  - Uncategorized
---
Was sharing a python script with a php developer to illustrate how a couple of requests can be made to an ArcGIS REST endpoint.

Nothing fancy&#8211;one gets the data for one record, one gets data for all records, and the final gets the data for the record that intersects a point.

The only thing to note is that the original version that I shared gets a token because the service it uses is secured. I commented out the routine to get a token and just substituted in a blank string and pointed the code at one of esri&#8217;s sample services at https://sampleserver6.arcgisonline.com/arcgis/.

The file can be downloaded <a href="https://github.com/MapRantala/Blog/blob/master/python/python/20180824_RESTRequests/sampleRESTRequests.py" target="_blank" rel="noopener">here</a>.

&nbsp;

<pre>import requests, urllib, httplib, urllib2, json

serverURL = 'https://sampleserver6.arcgisonline.com/arcgis/'
tokenURL =  serverURL+"tokens/"
phaseQueryURL = serverURL+"rest/services/Census/MapServer/3/query"

#Fields to be aware of:
# PhaseName = Name of the phase, a unique value.
# Status_Date = Date a phase was marked as "Live"
# PhaseStatus = Live, Provisioning, Pending

def makeRequest(inURL,inPayload):
    theRequest = urllib2.Request(url=inURL,data = urllib.urlencode(inPayload))
    theResponse = urllib2.urlopen(theRequest)
    responseString = theResponse.read()
    theJSON = json.loads(responseString)
    return theJSON

def getToken(inUser,inPass):
    inputData = {'username':inUser,'password':inPass, 'f':'json'}
    theJSON = makeRequest(tokenURL,inputData)
    print(theJSON)
    token = theJSON['token']
    return token

def queryAllStates(inToken):
    inputData = { 'where': "1=1", 'f': 'geojson', 'outFields': '*' , 'outSR': 4326, 'token':inToken}
    theJSON = makeRequest(phaseQueryURL,inputData)
    return theJSON

def queryState(inPhase,inToken):
    inputData = { 'where': "STATE_NAME = '{}'".format(inPhase), 'f': 'geojson', 'outFields': '*' , 'outSR': 4326, 'token':inToken}
    theJSON = makeRequest(phaseQueryURL,inputData)
    return theJSON

def findStateForPoint(inX,inY,inToken):
    inputData = {'geometry': { 'x': inX, 'y': inY}, 'geometryType': 'esriGeometryPoint', 'inSR':4326, 'spatialRel':'esriSpatialRelIntersects', 'outFields':'*', 'f': 'json','token':inToken}

    theJSON = makeRequest(phaseQueryURL,inputData)
    return theJSON

username = ""
password = ""

##theToken = getToken(username,password)
theToken = ""
print(theToken)

#Attribute Query to get information about one State
stateInfo = queryState('Wisconsin',theToken)
print(stateInfo)

#Attribute Query to get information about all States
statesInfo = queryAllStates(theToken)
print(statesInfo)


#Find phase for a lat-lon
theState = findStateForPoint(-93.280842,44.95531,theToken)
print(theState)
</pre>