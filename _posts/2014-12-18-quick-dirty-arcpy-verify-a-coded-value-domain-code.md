---
id: 4506
title: 'Quick &#038; Dirty Arcpy: Verify a Coded Value Domain Code'
date: 2014-12-18T22:32:11-06:00
author: Matt
layout: post
guid: http://maprantala.com/?p=4506
permalink: /2014/12/18/quick-dirty-arcpy-verify-a-coded-value-domain-code/
categories:
  - arcpy
  - ArcScripts
  - Python
  - 'Quick &amp; Dirty'
---
I&#8217;ve been working on a few different data import routines and one of the things I recently built was the ability to verify that a potential Code to be entered into a field with a <a href="http://resources.arcgis.com/en/help/main/10.2/index.html#//001s00000001000000" target="_blank">Coded Value Domain</a> is valid.

The logic of the code is pretty straight-forward. Get a field&#8217;s domain and check that a potential value is one of the code values. The biggest &#8220;trick&#8221; in this code is that <a href="http://resources.arcgis.com/en/help/main/10.2/index.html#//018w0000001z000000" target="_blank">arcpy.da.ListDomains</a>, which locates a field&#8217;s domain, takes a geodatabase (or Enterprise geodatabase connection file) as its only parameter. The documentation says it takes a workspace, but it does not like a feature dataset, which a feature class might be in.

A couple caveats about the code. It only returns True if a field exists, has a coded value domain, and the value tested is one of the (case-sensitive) valid codes. While I have an ArcToolbox tool to call it for illustration purposes, I&#8217;m only calling it from code so I wanted tight requirements. 

Anyhow, here is the code or <a href="https://github.com/MapRantala/Blog/tree/master/python/arcpy/20141218_VerifyDomainValue" target="_blank">download it</a> from GitHub.

<pre>import arcpy

inFeatureClass = sys.argv[1]
inField = sys.argv[2]
inValue = sys.argv[3]

# getFeatureClassParentWorkspace: This script gets the geodatabase for a
# feature class. The trick here is that feature classes can be within a
# feature dataset so you need to account for two possible levels in the
# directory structure.
def getFeatureClassParentWorkspace(inFeatureClass):
    describeFC = arcpy.Describe(inFeatureClass)
    if (describeFC.dataType == 'FeatureClass') or (describeFC.dataType == 'Table'):
        workspace1 = describeFC.path
        describeWorkspace1 = arcpy.Describe(workspace1)
        if (describeWorkspace1.dataType == 'FeatureDataset'):
            return describeWorkspace1.path
        return workspace1

    return None

# Find a field within a feature class
def getField(inFeatureClass, inFieldName):
  fieldList = arcpy.ListFields(inFeatureClass)
  for iField in fieldList:
    if iField.name.lower() == inFieldName.lower():
      return iField
  return None

#Get a field's domain
def getDomain(inFeatureClass, inField):
    theField = getField(inFeatureClass,inField)
    if (theField &lt;> None):
        searchDomainName = theField.domain
        if (searchDomainName &lt;> ""):
            for iDomain in arcpy.da.ListDomains(getFeatureClassParentWorkspace(inFeatureClass)):
                if iDomain.name == searchDomainName:
                    return iDomain
    return None

#Get the domain.
def validDomainValue(inFeatureClass,inField,inValue):
    theDomain = getDomain(inFeatureClass,inField)

    if not (theDomain is None):
        if (theDomain.domainType == "CodedValue"):
            if theDomain.codedValues.has_key(inValue):
                return True
    return False

if (validDomainValue(inFeatureClass,inField,inValue)):
    arcpy.AddMessage("Value ({0}) is valid for field [{1}].".format(inValue,inField))
else:
    arcpy.AddError("ERROR: Value ({0}) is invalid for field [{1}].".format(inValue,inField))
</pre>