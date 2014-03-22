---
layout: post
status: publish
published: true
title: Speed Up and Version Your Views
author: Steven Merrill
author_login: smerrill
author_email: info@phase2technology.com
excerpt: ! '<p>Since getting started with Drupal over two years ago, the sites I&rsquo;ve
  built with it have naturally gotten bigger and bigger in scope. As your sites get
  bigger and bigger, you always look for ways to keep your site running as smoothly
  as possible, and this usually ends up meaning getting rid of queries wherever you
  can.</p>

  <p>One feature of Views which is often used by module developers is the ability
  of a module to expose a set of <strong>default views</strong>. The calendar module,
  for example, provides a default calendar view in both its Drupal 5 and 6 versions.  This
  is an obvious asset for developers of contributed modules: if your module interfaces
  with Views, it makes sense to provide a default view that users can modify.</p>

  <h2>Implementation and Potential Benefits</h2>

  <p>The hook used to do this is <a href="http://views-help.doc.logrus.com/help/views/api-default-views">hook_views_default_views()</a>.
  Since most modern sites are run off of a number of Views, you can also realize several
  benefits by building your sites with a custom module that implements <strong>hook_views_default_views()</strong>:</p>

  <ol>

  <li>It improves performance. Views implemented via hook_views_default_views() do
  not require a database query to instantiate. You will realize an even greater performance
  gain if you also use an opcode caching system such as APC or XCache.</li>

  <li>Providing views in code will allow you to override the default view to make
  changes, and you can then choose to keep the changes in the database, update the
  module file to reflect the changes, or revert back to the version in your module
  file.</li>

  <li>Because the view (and theoretically, changes to that view) are stored in files,
  you can put them in version control and see how the views used in your site change
  over time and revert to an earlier version without having to go to a database backup.</li>

  </ol>'
wordpress_id: 1184
wordpress_url: http://phase2technology.com/speed-up-and-version-your-views/
date: !binary |-
  MjAwOC0xMS0wNSAxNTowNToxOSAtMDUwMA==
date_gmt: !binary |-
  MjAwOC0xMS0wNSAyMDowNToxOSAtMDUwMA==
categories:
- Uncategorized
tags:
- development
- performance
- planet drupal
- tutorials
- views
comments: []
---
<p>Since getting started with Drupal over two years ago, the sites I&rsquo;ve built with it have naturally gotten bigger and bigger in scope. As your sites get bigger and bigger, you always look for ways to keep your site running as smoothly as possible, and this usually ends up meaning getting rid of queries wherever you can.<&#47;p></p>
<p>One feature of Views which is often used by module developers is the ability of a module to expose a set of <strong>default views<&#47;strong>. The calendar module, for example, provides a default calendar view in both its Drupal 5 and 6 versions.  This is an obvious asset for developers of contributed modules: if your module interfaces with Views, it makes sense to provide a default view that users can modify.<&#47;p></p>
<h2>Implementation and Potential Benefits<&#47;h2></p>
<p>The hook used to do this is <a href="http:&#47;&#47;views-help.doc.logrus.com&#47;help&#47;views&#47;api-default-views">hook_views_default_views()<&#47;a>. Since most modern sites are run off of a number of Views, you can also realize several benefits by building your sites with a custom module that implements <strong>hook_views_default_views()<&#47;strong>:<&#47;p></p>
<ol>
<li>It improves performance. Views implemented via hook_views_default_views() do not require a database query to instantiate. You will realize an even greater performance gain if you also use an opcode caching system such as APC or XCache.<&#47;li>
<li>Providing views in code will allow you to override the default view to make changes, and you can then choose to keep the changes in the database, update the module file to reflect the changes, or revert back to the version in your module file.<&#47;li>
<li>Because the view (and theoretically, changes to that view) are stored in files, you can put them in version control and see how the views used in your site change over time and revert to an earlier version without having to go to a database backup.<&#47;li><br />
<&#47;ol></p>
<p><!--break--><&#47;p></p>
<h2>Views2 &#47; Drupal 6 Implementation<&#47;h2></p>
<p>Here&rsquo;s how to do it in Views2 on Drupal 6. When implementing a site, we at Tree House usually end up with a utility module that will do some of the things that will need to do theming or development tweaks, like the odd hook_form_alter(), or provide a Views handler or two. I&rsquo;ll be calling this module treehouse_utils.  There&rsquo;s two steps to providing a default view in Drupal 6 and Views2: you need to tell Views2 that you implement the Views2 API, and then you have to create a file that contains the default view(s).<&#47;p></p>
<p>So first, in treehouse_utils.module, it&rsquo;s time to tells Views2 that we&rsquo;re implementing Views features with <strong>hook_views_api()<&#47;strong>. The implementation of the hook looks like this in our module:<&#47;p></p>
<p>
<div class="geshifilter">
<div class="php geshifilter-php"><span class="kw2">function<&#47;span> treehouse_utils_views_api<span class="br0">&#40;<&#47;span><span class="br0">&#41;<&#47;span> <span class="br0">&#123;<&#47;span><br &#47;><br />
&nbsp; <span class="kw1">return<&#47;span> <a href="http:&#47;&#47;www.php.net&#47;array"><span class="kw3">array<&#47;span><&#47;a><span class="br0">&#40;<&#47;span><br &#47;><br />
&nbsp; &nbsp; <span class="st_h">'api'<&#47;span> <span class="sy0">=&amp;<&#47;span>gt<span class="sy0">;<&#47;span> <span class="nu0">2<&#47;span><span class="sy0">,<&#47;span><br &#47;><br />
&nbsp; &nbsp; <span class="st_h">'path'<&#47;span> <span class="sy0">=&amp;<&#47;span>gt<span class="sy0">;<&#47;span> drupal_get_path<span class="br0">&#40;<&#47;span><span class="st_h">'module'<&#47;span><span class="sy0">,<&#47;span> <span class="st_h">'treehouse_utils'<&#47;span><span class="br0">&#41;<&#47;span><span class="sy0">,<&#47;span><br &#47;><br />
&nbsp; <span class="br0">&#41;<&#47;span><span class="sy0">;<&#47;span><br &#47;><br />
<span class="br0">&#125;<&#47;span><&#47;div><&#47;div><&#47;p></p>
<p>This module just tells Views2 that we&rsquo;re implementing version 2 of the Views API and that it should look for Views-related files in the directory of the <strong>treehouse_utils<&#47;strong> module. (Some other modules will make a separate directory called <em>includes<&#47;em> where its Views-related files will live.)<&#47;p></p>
<h2>Putting the Views in Code<&#47;h2></p>
<p>The other task that we need to do is to implement <strong>hook_views_default_views()<&#47;strong> and actually tell Drupal about our Views. It is possible to do this in the main treehouse_utils.module file, but the trend in Drupal 6 is towards using more files so that Drupal has less code to load and parse overall on each page request. You can implement <strong>hook_views_default_views()<&#47;strong> (as well as a smattering of other Views-related hooks) in a <em>MODULENAME.views.inc<&#47;em> file. In the case of <strong>hook_views_default_views()<&#47;strong>, you can implement that in its own file, <em>MODULENAME.views_default.inc<&#47;em>.<&#47;p></p>
<p>So here&rsquo;s what treehouse_utils.views_default.inc will look like:<&#47;p></p>
<p>
<div class="geshifilter">
<div class="php geshifilter-php"><span class="sy0">&amp;<&#47;span>lt<span class="sy0">;<&#47;span>?php<br &#47;><br />
<br &#47;><br />
<span class="kw2">function<&#47;span> treehouse_utils_views_default_views<span class="br0">&#40;<&#47;span><span class="br0">&#41;<&#47;span> <span class="br0">&#123;<&#47;span><br &#47;><br />
&nbsp; <span class="re0">$views<&#47;span> <span class="sy0">=<&#47;span> <a href="http:&#47;&#47;www.php.net&#47;array"><span class="kw3">array<&#47;span><&#47;a><span class="br0">&#40;<&#47;span><span class="br0">&#41;<&#47;span><span class="sy0">;<&#47;span><br &#47;><br />
<br &#47;><br />
&nbsp; <span class="co1">&#47;&#47; Start copy and paste of Export tab output.<&#47;span><br &#47;><br />
<br &#47;><br />
&nbsp; <span class="co1">&#47;&#47; End copy and paste of Export tab output.<&#47;span><br &#47;><br />
<br &#47;><br />
&nbsp; <span class="co1">&#47;&#47; Add view to list of views to provide.<&#47;span><br &#47;><br />
&nbsp; <span class="re0">$views<&#47;span><span class="br0">&#91;<&#47;span><span class="re0">$view<&#47;span><span class="sy0">-&amp;<&#47;span>gt<span class="sy0">;<&#47;span>name<span class="br0">&#93;<&#47;span> <span class="sy0">=<&#47;span> <span class="re0">$view<&#47;span><span class="sy0">;<&#47;span><br &#47;><br />
<br &#47;><br />
&nbsp; <span class="co1">&#47;&#47; ...Repeat all of the above for each view the module should provide.<&#47;span><br &#47;><br />
<br &#47;><br />
&nbsp; <span class="co1">&#47;&#47; At the end, return array of default views.<&#47;span><br &#47;><br />
&nbsp; <span class="kw1">return<&#47;span> <span class="re0">$views<&#47;span><span class="sy0">;<&#47;span><br &#47;><br />
<span class="br0">&#125;<&#47;span><&#47;div><&#47;div><&#47;p></p>
<p>Implementations of <strong>hook_views_default_views()<&#47;strong> are expected to return an array, so for each view that you would like to have provided by a module, go to the Views2 administrative interface, click Export, and copy and paste the PHP code between the two comments, and also be sure to include the $views[$view->name] = $view; line, where the newly-defined view object gets included in the array that will be returned.<&#47;p></p>
<p>Finally, when your module is enabled (or when you next visit the Views2 administration page or otherwise clear Views2&rsquo;s caches,) you&rsquo;ll see the following:<&#47;p></p>
<p><img src="http:&#47;&#47;img.skitch.com&#47;20081105-tiq92fi3msh5dmf3pruy2mugxm.png" &#47;><&#47;p></p>
<p>Notice that because the view which I put into the module is also in the database, Views tells us that the view is <em>overriden<&#47;em> - a default is now provided in PHP code, but our version in the database still supercedes it.  If you were to click <strong>Revert<&#47;strong>, the database entry for the view would be deleted, and Drupal would load it from your <em>MODULENAME.views_default.inc<&#47;em> file.<&#47;p></p>
<h2>Continuing this Workflow<&#47;h2></p>
<p>At this point, you can check your module into version control and have a record of your view at this point in time.  Later, you could make some more changes in the Views2 UI, hit <strong>Export<&#47;strong> again, and paste the newer version into the file, check that into version control and continue.<&#47;p></p>
<p>Views2 is amazing, and these techniques let you squeeze a little more performance and a lot more possibilities for revision control while building out your sites.<&#47;p></p>
<p>Also, be sure to vote for <a href="http:&#47;&#47;dc2009.drupalcon.org&#47;session&#47;scaling-drupal-not-ifhow">Thomas&rsquo;s DrupalCon session<&#47;a> if you&rsquo;d like to look at concrete techniques of all types to scale your Drupal sites!<&#47;p></p>
