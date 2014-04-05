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
categories:
- development
- drupal
- drupal
- drupal planet
- jquery
- php
- documentation
- dash
- mysql
comments: []
---
<p>Wouldn't it be great if there was an easy way to access php.net or other documentation offline or on a plane?</p></p>
<p><strong>UPDATE:</strong> Sadly, as this blog post went to press, two important updates came out that change the usefulness of this blog post. Dash is now ad-supported, and secondly, it ships with a Drupal DocSet available for download, so that's one fewer step you have to perform to have all the docs that matter to you in Dash.</p></p>
<p>There's a free as in beer application called Dash (available on the Mac App Store at <a href="http://itunes.apple.com/us/app/dash/id458034879?ls=1&mt=12" title="http://itunes.apple.com/us/app/dash/id458034879?ls=1&mt=12">http://itunes.apple.com/us/app/dash/id458034879?ls=1&mt=12</a>) available for Mac OS X. Dash is a nice-looking documentation browser featuring several useful features, such as the ability to query it with a custom URL string (dash://YOURQUERY), which lends itself for use in tools like Alfred.</p></p>
<p>Dash can also download additional documentation sets for many open source technologies, including MySQL, PHP, and jQuery. It can be handy to search through the latest PHP API documentation no matter what kind of connection you're on, like so:</p><br />
<a href="https://skitch.com/00sven/87u7b/dash-documentation"><img src="https://img.skitch.com/20120530-ttupxms7g3b48xep9yu3qk8rs2.medium.jpg" alt="Dash - Documentation" /></a></p>
<p>In addition, Dash also has the ability to browse any API documentation that you have installed through XCode onto your system. (In fact, any files in DocSet format that are located inside the ~/Library/Developer/Shared/Documentation/DocSets directory can be read by Dash.)</p></p>
<p>In addition to the freely available DocSets that are available for major open-source technologies, it's easy to make your own DocSets using doxygen. I went ahead and made a DocSet for Drupal 7.x using doxygen. Not every method that's available at api.drupal.org is here, but it's a great start, especially if you want a single offline app where you can query offline documentation.</p></p>
<p>To start using the Drupal DocSet, download the .tgz file from <a href="https://github.com/phase2/drupal-docset/zipball/master" title="https://github.com/phase2/drupal-docset/zipball/master">https://github.com/phase2/drupal-docset/zipball/master</a>. To use it:</p></p>
<ol>
<li>Unzip the file</li>
<li>Move org.drupal.docset to ~/Library/Developer/Shared/Documentation/DocSets/</li>
<li>Launch Dash and start searching, like so.</li><br />
</ol><br />
<a href="https://skitch.com/00sven/87u44/dash-documentation"><img src="https://img.skitch.com/20120530-rrgw89tfht2g7ie3ejys98xssr.medium.jpg" alt="Dash - Documentation" /></a></p>
