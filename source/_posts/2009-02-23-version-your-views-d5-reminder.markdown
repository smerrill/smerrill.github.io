---
layout: post
status: publish
published: true
title: ! 'Version Your Views: D5 Reminder'
author: Steven Merrill
author_login: smerrill
author_email: info@phase2technology.com
excerpt: ! '<p>In <a href="http://treehouseagency.com/blog/steven-merrill/2008/11/05/speed-and-version-your-views">a
  previous article</a> I extolled the virtues of keeping your Views in code, which
  lets you deploy or change them as easily as uploading or updating a module on your
  production site. In the article, I wrote about using Drupal 6 and Views2 to do so.</p>

  <h3>Drupal 5 / Views 1 Execution</h3>

  <p>Views 1 for Drupal 5 also has this mechanism in place, so if you''re working
  on a Drupal 5 site, it''s still worth using. It&rsquo;s how the calendar and date
  modules provide a default calendar view, among other things.  The method for putting
  default views in your modules is pretty similar, and it&rsquo;s still a great technique
  to practice.  In Views 2 for Drupal 6, you had to implement two hooks to have a
  module provide default views in code: hook_views_api() and hook_views_default_views().</p>'
wordpress_id: 1181
wordpress_url: http://www.phase2technology.com/version-your-views-d5-reminder/
date: !binary |-
  MjAwOS0wMi0yMyAxNzowNTo0MCAtMDUwMA==
date_gmt: !binary |-
  MjAwOS0wMi0yMyAyMjowNTo0MCAtMDUwMA==
categories:
- Uncategorized
tags:
- development
- drupal
- views
comments: []
---
<p>In <a href="http:&#47;&#47;treehouseagency.com&#47;blog&#47;steven-merrill&#47;2008&#47;11&#47;05&#47;speed-and-version-your-views">a previous article<&#47;a> I extolled the virtues of keeping your Views in code, which lets you deploy or change them as easily as uploading or updating a module on your production site. In the article, I wrote about using Drupal 6 and Views2 to do so.<&#47;p></p>
<h3>Drupal 5 &#47; Views 1 Execution<&#47;h3></p>
<p>Views 1 for Drupal 5 also has this mechanism in place, so if you're working on a Drupal 5 site, it's still worth using. It&rsquo;s how the calendar and date modules provide a default calendar view, among other things.  The method for putting default views in your modules is pretty similar, and it&rsquo;s still a great technique to practice.  In Views 2 for Drupal 6, you had to implement two hooks to have a module provide default views in code: hook_views_api() and hook_views_default_views().<&#47;p></p>
<p>Views 1 doesn&rsquo;t have the hook_views_api(), nor will Views 1 automatically look for a MODULENAME.views.inc or a MODULENAME.views_default.inc file, so you can just put an implementation of hook_views_default_views() in your main module file.  If we were making the fictitious treehouse_utils module, the code would look exactly the same as in the previous example:<&#47;p></p>
<p>
<div class="geshifilter">
<div class="php geshifilter-php"><span class="sy0">&amp;<&#47;span>lt<span class="sy0">;<&#47;span>?php<br &#47;><br />
<span class="kw2">function<&#47;span> treehouse_utils_views_default_views<span class="br0">&#40;<&#47;span><span class="br0">&#41;<&#47;span> <span class="br0">&#123;<&#47;span><br &#47;><br />
<span class="re0">$views<&#47;span> <span class="sy0">=<&#47;span> <a href="http:&#47;&#47;www.php.net&#47;array"><span class="kw3">array<&#47;span><&#47;a><span class="br0">&#40;<&#47;span><span class="br0">&#41;<&#47;span><span class="sy0">;<&#47;span><br &#47;><br />
<br &#47;><br />
<span class="co1">&#47;&#47; Start copy and paste of Export tab output.<&#47;span><br &#47;><br />
<br &#47;><br />
<span class="co1">&#47;&#47; End copy and paste of Export tab output.<&#47;span><br &#47;><br />
<br &#47;><br />
<span class="co1">&#47;&#47; Add view to list of views to provide.<&#47;span><br &#47;><br />
<span class="re0">$views<&#47;span><span class="br0">&#91;<&#47;span><span class="re0">$view<&#47;span><span class="sy0">-&amp;<&#47;span>gt<span class="sy0">;<&#47;span>name<span class="br0">&#93;<&#47;span> <span class="sy0">=<&#47;span> <span class="re0">$view<&#47;span><span class="sy0">;<&#47;span><br &#47;><br />
<br &#47;><br />
<span class="co1">&#47;&#47; ...Repeat all of the above for each view the module should provide.<&#47;span><br &#47;><br />
<br &#47;><br />
<span class="co1">&#47;&#47; At the end, return array of default views.<&#47;span><br &#47;><br />
<span class="kw1">return<&#47;span> <span class="re0">$views<&#47;span><span class="sy0">;<&#47;span><br &#47;><br />
<span class="br0">&#125;<&#47;span><&#47;div><&#47;div><&#47;p></p>
<h3>Activating Your Views<&#47;h3></p>
<p>Unlike Views2, default views are updated when a module is first enabled, so if you are adding default views to an existing module, you will have to disable and re-enable the module to get them to show up, or clear out the Views cache, which you can do by going to <a href="http:&#47;&#47;yoursite.com&#47;admin&#47;build&#47;views&#47;tools" title="http:&#47;&#47;yoursite.com&#47;admin&#47;build&#47;views&#47;tools">http:&#47;&#47;yoursite.com&#47;admin&#47;build&#47;views&#47;tools<&#47;a> and clicking the Clear Views Cache button.<&#47;p></p>
<p>The Clear Views Cache approach is a far cleaner one than disabling and reenabling modules. I recently put some default views for Drupal 5 in a custom module which was the backbone of our site, and disabling and re-enabling it was no easy task.<&#47;p></p>
