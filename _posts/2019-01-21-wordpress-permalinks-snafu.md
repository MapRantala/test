---
id: 4843
title: WordPress Permalinks Snafu
date: 2019-01-21T05:19:03-06:00
author: adminguy
layout: post
guid: https://maprantala.com/?p=4843
permalink: /2019/01/21/wordpress-permalinks-snafu/
image: /wp-content/uploads/2019/01/stats.png
categories:
  - Wordpress
tags:
  - Wordpress
---
Using a free <a rel="noreferrer noopener" aria-label=" (opens in a new tab)" href="http://statcounter.com/" target="_blank">StatCounter.com</a> add-in, I get weekly emails showing the number of visitors to this blog. During the summer, the numbers plummeted. At the time, I thought it had to do with Google/Chrome requiring https.<figure class="wp-block-image">

<img src="https://i0.wp.com/maprantala.com/wp-content/uploads/2019/01/stats.png?w=1170&#038;ssl=1" alt="" class="wp-image-4844" srcset="https://i0.wp.com/maprantala.com/wp-content/uploads/2019/01/stats.png?w=546&ssl=1 546w, https://i0.wp.com/maprantala.com/wp-content/uploads/2019/01/stats.png?resize=300%2C138&ssl=1 300w" sizes="(max-width: 546px) 100vw, 546px" data-recalc-dims="1" /> </figure> 

Luckily, HostGator (disclosure: I&#8217;m working on becoming a HostGator affiliate), who I use to host this blog and a few other websites, provided free https certificates for all of my sites. So I made sure that https://MapRantala.com worked and thought the numbers would rebound.

Looking at the chart, you can see they did only slightly. I assumed that it would just be a matter of time for Google to see that https was enabled and since the blog is more a place for me to make notes to myself, I wasn&#8217;t concerned.

This weekend, I decided to take a look at Google&#8217;s Search Console and see if I could determine what was going on. Looking in the coverage, I saw many 404 results for pages. As I investigated some of them, I realized that it was due to my Permalinks setting. 

The URLs, as shown below, were not working.<figure class="wp-block-image">

<img src="https://i0.wp.com/maprantala.com/wp-content/uploads/2019/01/404.jpg?w=1170&#038;ssl=1" alt="" class="wp-image-4846" srcset="https://i0.wp.com/maprantala.com/wp-content/uploads/2019/01/404.jpg?w=836&ssl=1 836w, https://i0.wp.com/maprantala.com/wp-content/uploads/2019/01/404.jpg?resize=300%2C198&ssl=1 300w, https://i0.wp.com/maprantala.com/wp-content/uploads/2019/01/404.jpg?resize=768%2C506&ssl=1 768w" sizes="(max-width: 836px) 100vw, 836px" data-recalc-dims="1" /> </figure> 

The website was using &#8220;https://maprantala.com/index.php/&#8221; as the base URL. So, for example, &#8220;https://maprantala.com/2017/08/07/arcgis-pro-2-0-migration-overview&#8221; did not work but &#8220;https://maprantala.com/**index.php**/2017/08/07/arcgis-pro-2-0-migration-overview&#8221; did. 

To change the WordPress Permalinks setting, I went to Settings-Permalinks and removed he &#8220;index.php/&#8221; from the Custom Structure. Waiting a day, the 404s have disappeared in Search Console and, in theory, I just need to wait for Google to re-index the pages which are currently shown as &#8220;Crawled &#8211; currently not indexed&#8221;<figure class="wp-block-image">

<img src="https://i2.wp.com/maprantala.com/wp-content/uploads/2019/01/permalinks.jpg?w=1170&#038;ssl=1" alt="Wordpress Permalinks Setting " class="wp-image-4847" srcset="https://i2.wp.com/maprantala.com/wp-content/uploads/2019/01/permalinks.jpg?w=961&ssl=1 961w, https://i2.wp.com/maprantala.com/wp-content/uploads/2019/01/permalinks.jpg?resize=300%2C45&ssl=1 300w, https://i2.wp.com/maprantala.com/wp-content/uploads/2019/01/permalinks.jpg?resize=768%2C115&ssl=1 768w" sizes="(max-width: 961px) 100vw, 961px" data-recalc-dims="1" /> <figcaption>WordPress Permalinks Setting </figcaption></figure> 

That may not have been the whole problem. I republished this blog under a different name for a year. But moved it back to this domain when that domain registration expired. So that may have been another factor.