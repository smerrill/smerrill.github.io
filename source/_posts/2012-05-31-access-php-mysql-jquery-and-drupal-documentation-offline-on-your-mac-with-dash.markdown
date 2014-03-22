---
layout: post
status: publish
published: true
title: Access PHP, MySQL, jQuery, and Drupal documentation offline on your Mac with
  Dash
author: Steven Merrill
author_login: smerrill
author_email: info@phase2technology.com
excerpt: ! '<p>Wouldn''t it be great if there was an easy way to access php.net or
  other documentation offline or on a plane?</p>

  <p><strong>UPDATE:</strong> Sadly, as this blog post went to press, two important
  updates came out that change the usefulness of this blog post. Dash is now ad-supported,
  and secondly, it ships with a Drupal DocSet available for download, so that''s one
  fewer step you have to perform to have all the docs that matter to you in Dash.</p>'
wordpress_id: 562
wordpress_url: http://www.phase2technology.com/access-php-mysql-jquery-and-drupal-documentation-offline-on-your-mac-with-dash/
date: !binary |-
  MjAxMi0wNS0zMSAxNToyODozMyAtMDQwMA==
date_gmt: !binary |-
  MjAxMi0wNS0zMSAyMDoyODozMyAtMDQwMA==
categories:
- Development
- Drupal
tags:
- drupal
- drupal planet
- jquery
- php
- documentation
- dash
- mysql
comments: []
---
<p>Wouldn't it be great if there was an easy way to access php.net or other documentation offline or on a plane?<&#47;p></p>
<p><strong>UPDATE:<&#47;strong> Sadly, as this blog post went to press, two important updates came out that change the usefulness of this blog post. Dash is now ad-supported, and secondly, it ships with a Drupal DocSet available for download, so that's one fewer step you have to perform to have all the docs that matter to you in Dash.<&#47;p></p>
<p>There's a free as in beer application called Dash (available on the Mac App Store at <a href="http:&#47;&#47;itunes.apple.com&#47;us&#47;app&#47;dash&#47;id458034879?ls=1&amp;mt=12" title="http:&#47;&#47;itunes.apple.com&#47;us&#47;app&#47;dash&#47;id458034879?ls=1&amp;mt=12">http:&#47;&#47;itunes.apple.com&#47;us&#47;app&#47;dash&#47;id458034879?ls=1&amp;mt=12<&#47;a>) available for Mac OS X. Dash is a nice-looking documentation browser featuring several useful features, such as the ability to query it with a custom URL string (dash:&#47;&#47;YOURQUERY), which lends itself for use in tools like Alfred.<&#47;p></p>
<p>Dash can also download additional documentation sets for many open source technologies, including MySQL, PHP, and jQuery. It can be handy to search through the latest PHP API documentation no matter what kind of connection you're on, like so:<&#47;p><br />
<a href="https:&#47;&#47;skitch.com&#47;00sven&#47;87u7b&#47;dash-documentation"><img src="https:&#47;&#47;img.skitch.com&#47;20120530-ttupxms7g3b48xep9yu3qk8rs2.medium.jpg" alt="Dash - Documentation" &#47;><&#47;a></p>
<p>In addition, Dash also has the ability to browse any API documentation that you have installed through XCode onto your system. (In fact, any files in DocSet format that are located inside the ~&#47;Library&#47;Developer&#47;Shared&#47;Documentation&#47;DocSets directory can be read by Dash.)<&#47;p></p>
<p>In addition to the freely available DocSets that are available for major open-source technologies, it's easy to make your own DocSets using doxygen. I went ahead and made a DocSet for Drupal 7.x using doxygen. Not every method that's available at api.drupal.org is here, but it's a great start, especially if you want a single offline app where you can query offline documentation.<&#47;p></p>
<p>To start using the Drupal DocSet, download the .tgz file from <a href="https:&#47;&#47;github.com&#47;phase2&#47;drupal-docset&#47;zipball&#47;master" title="https:&#47;&#47;github.com&#47;phase2&#47;drupal-docset&#47;zipball&#47;master">https:&#47;&#47;github.com&#47;phase2&#47;drupal-docset&#47;zipball&#47;master<&#47;a>. To use it:<&#47;p></p>
<ol>
<li>Unzip the file<&#47;li>
<li>Move org.drupal.docset to ~&#47;Library&#47;Developer&#47;Shared&#47;Documentation&#47;DocSets&#47;<&#47;li>
<li>Launch Dash and start searching, like so.<&#47;li><br />
<&#47;ol><br />
<a href="https:&#47;&#47;skitch.com&#47;00sven&#47;87u44&#47;dash-documentation"><img src="https:&#47;&#47;img.skitch.com&#47;20120530-rrgw89tfht2g7ie3ejys98xssr.medium.jpg" alt="Dash - Documentation" &#47;><&#47;a></p>
