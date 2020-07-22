---
id: 340
title: Multiple geodatabases and ArcSDE services from one PostgreSQL server.
date: 2011-02-17T18:22:07-06:00
author: Matt
excerpt: Attempting to create a second database and ArcSDE service on a PostgreSQL server displays an error message but is actually successful.
layout: post
guid: http://maprantala.com/?p=340
permalink: /2011/02/17/multiple-geodatabases-and-arcsde-services-from-one-postgresql-server/
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
  - ArcGIS 10
  - ArcSDE
  - Bugs
  - ESRI
tags:
  - arcgis
  - arcgis 10
  - ArcSDE
  - ArcSDE Post Installation
  - ArcSDE Service
  - ESRI
  - PostGreSQL
  - Windows Server 2008 R2
---
To better organize our ArcSDE data, we wanted to create multiple geodatabases and multiple ArcSDE services using one PostgreSQL database cluster (a cluster containing 1 machine at this point).  A side question is why can&#8217;t tables and raster be placed in Feature Datasets?  This wouldn&#8217;t be an end-all solution for what we want to do but there are some messy consequences of this limitation.

ESRI has instructions on [Setting up multiple geodatabases in one PostgreSQL database cluster on Windows](http://help.arcgis.com/EN/ArcGISDesktop/10.0/Help/index.html#//002p00000011000000.htm) which was helpful but we repeatedly got an &#8220;The ArcSDE Repository was unsuccessfully completed.  Would you like to view this error?&#8221; error during the Repository Setup step, step two of four, of the ArcSDE Post Installation process. The odd thing, however, was that the wise_err.log was blank so I had no clue on why it was failing.  After much Googling and head-banging, I came across a [post from](http://forums.esri.com/Thread.asp?c=158&f=2290&t=298474#941273) [Kim Doty](http://www.linkedin.com/profile/view?id=24468141&authType=name&authToken=VoHW&pvs=pp&trk=ppro_viewmore) on ESRI&#8217;s forums reporting the same problem but they were able to create a service by just continuing through the process .  Figuring I had nothing to lose, I thought I might as well try to go through the entire ArcSDE Post Installation process and see what happened.  Although I received blank error messages, I was able to successfully create the second ArcSDE instance.

I will touch on some of the highlights of the process.  In my case, I was creating a database & service with these parameters:

  * Database name:![](/Users/mrantala/AppData/Local/Temp/moz-screenshot-11.png) mgsgdb_lidar
  * Service name:  mgs1_sde
  * Tablespace folder: D:mgsdb1dmgsgdb_lidar

Following ESRI&#8217;s instructions, I first made custom copies of dbinit.sde, dbtune.sde, and giomgr.defs that contain the service name (mgs1\_sde) within the file name.  These copies were named dbinit\_mgs1\_sde.sde, dbtune\_mgs1\_sde.sde, and giomgr\_mgs1_sde.defs.

[<img class="alignnone size-full wp-image-347" title="Custom Files" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2011/02/files1.jpg?resize=360%2C228" alt="" width="360" height="228" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2011/02/files1.jpg)

When I have trouble with the ArcSDE Post Installation, I like to go through the four steps individually so I select a &#8220;Custom&#8221; install.

[<img class="alignnone size-full wp-image-343" title="Custom Setup" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2011/02/custom.jpg?resize=519%2C391" alt="" width="519" height="391" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2011/02/custom.jpg)

The four steps making up the ArcSDE Post Installation process are shown on the next dialog.

[<img class="alignnone size-full wp-image-344" title="Individual Steps" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/02/steps.jpg?resize=514%2C392" alt="" width="514" height="392" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/02/steps.jpg)

I was able to complete the first step, Define SDE User Environment, without any problems.

Make sure to set your database name, default tablespace, and tablespace folders are distinct from your first installation.

[<img class="alignnone size-full wp-image-345" title="Database name and workspace" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2011/02/databaseinfo.jpg?resize=511%2C389" alt="" width="511" height="389" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2011/02/databaseinfo.jpg)

During the second step of the ArcSDE Post Installation process, Repository Setup, make sure to use the custom files you made earlier when you come to the ArcSDE configuratino files dialog.

[<img class="alignnone size-full wp-image-349" title="Custom Files" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/02/customfiles.jpg?resize=516%2C393" alt="" width="516" height="393" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/02/customfiles.jpg)

Later in the Repository Setup, make sure to use the correct database name:

[<img class="alignnone size-full wp-image-350" title="Step2, Set Database Name" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/02/step2-databasename.jpg?resize=515%2C389" alt="" width="515" height="389" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/02/step2-databasename.jpg)

This is the first error message I received in the process (still during Repository Setup):

[<img class="alignnone size-full wp-image-351" title="View Error" src="https://i0.wp.com/maprantala.com/wp-content/uploads/2011/02/viewerror.jpg?resize=322%2C165" alt="" width="322" height="165" data-recalc-dims="1" />](https://i0.wp.com/maprantala.com/wp-content/uploads/2011/02/viewerror.jpg)

Clicking &#8220;Yes&#8221;, however, shows an empty wise_err.log file:

[<img class="alignnone size-full wp-image-352" title="Empty Log" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2011/02/emptylog.jpg?resize=630%2C335" alt="" width="630" height="335" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2011/02/emptylog.jpg)

After viewing this, I canceled out of the Repository Setup step.

Taking a look in pgAdmin III, however, I can see that both my database (mgsgdb\_lidar) and tablespace (mgsgdb\_lidar) were created:

[<img class="alignnone size-full wp-image-353" title="pgAdminIII" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/02/step2-inpgadminiii.jpg?resize=384%2C342" alt="" width="384" height="342" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/02/step2-inpgadminiii.jpg)

I received the same error message during the third step, Authorize ArcSDE, as in the second step:

[<img class="alignnone size-full wp-image-355" title="Step 3 Error" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/02/step3error.jpg?resize=320%2C160" alt="" width="320" height="160" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/02/step3error.jpg)

But running the fourth step, Create ArcSDE Service, resulted in this sweet message:

[<img class="alignnone size-full wp-image-356" title="Success" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/02/success.jpg?resize=232%2C145" alt="" width="232" height="145" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/02/success.jpg)

After editing my pg_hba.conf and opening the port in my firewall, the service was created, running, and visible.  So far, it seems to be fully functional and without problems.

<div id="geo-post-340" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>