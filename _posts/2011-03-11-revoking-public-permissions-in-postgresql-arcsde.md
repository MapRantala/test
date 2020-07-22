---
id: 428
title: 'Revoking &#8216;Public&#8217; Permissions in PostgreSQL ArcSDE'
date: 2011-03-11T11:11:48-06:00
author: Matt
excerpt: "Revoking 'public' privileges to an ArcSDE datasource in PGadmin III"
layout: post
guid: http://maprantala.com/?p=428
permalink: /2011/03/11/revoking-public-permissions-in-postgresql-arcsde/
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
  - ArcGIS 10
  - ArcSDE
  - PostgreSQL
tags:
  - ArcSDE
  - arcsde 10
  - arcsde postgresql
  - pgadmin III
---
While banging my head on how to grant access to a [referenced mosaic dataset](http://wp.me/pVrsJ-6Q) , I did something out of frustration that I normally would not do&#8211;I granted &#8216;public&#8217; access to some data.

Then, after figuring out the problem, I went to revoke public access using ArcCatalog and received this error message:

Error 999999: Error executing function.

The Object being referenced does not exist [ERROR:  role &#8220;public&#8221; does not exist::SQL state: 42704]  
Failed to execute (ChangePrivileges).

[<img class="alignnone size-full wp-image-430" title="Revoke Public Access in ArcCatalog" src="https://i1.wp.com/maprantala.com/wp-content/uploads/2011/03/revokepublic.jpg?resize=630%2C336" alt="" width="630" height="336" data-recalc-dims="1" />](https://i1.wp.com/maprantala.com/wp-content/uploads/2011/03/revokepublic.jpg)

I actually expected that would occur&#8211;ArcSDE isn&#8217;t aware of PostgreSQL&#8217;s public account.

The solution was to go into pgAdmin II and revoke the privileges there.

[<img class="aligncenter size-full wp-image-431" title="Revoke public access in PGadminIII" src="https://i2.wp.com/maprantala.com/wp-content/uploads/2011/03/revokepgadminiii.jpg?resize=442%2C576" alt="" width="442" height="576" data-recalc-dims="1" />](https://i2.wp.com/maprantala.com/wp-content/uploads/2011/03/revokepgadminiii.jpg)

<div id="geo-post-428" class="geo geo-post" style="display: none">
  <span class="latitude">44.852994</span><span class="longitude">-93.55073</span>
</div>