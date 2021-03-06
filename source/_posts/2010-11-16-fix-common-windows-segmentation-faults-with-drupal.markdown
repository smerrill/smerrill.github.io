---
layout: post
status: publish
published: true
title: Fix Common Windows Segmentation Faults with Drupal
author: Steven Merrill
author_login: smerrill
author_email: info@phase2technology.com
excerpt: <p>At Treehouse Agency, we often work with internal development teams, and
  enterprise software being what it is, they often run Windows. This has been the
  primary driver behind some of our technology choices (using Mercurial rather than
  Git on these sorts of projects) and it also occasionally necessitates some extra
  debugging when something doesn't quite work right on Windows.</p>
wordpress_id: 1157
wordpress_url: http://www.phase2technology.com/fix-common-windows-segmentation-faults-with-drupal/
categories:
- segmentation fault
- wamp
- windows
- phase2blog
comments: []
---
<p>At Treehouse Agency, we often work with internal development teams, and enterprise software being what it is, they often run Windows. This has been the primary driver behind some of our technology choices (using Mercurial rather than Git on these sorts of projects) and it also occasionally necessitates some extra debugging when something doesn't quite work right on Windows.</p></p>
<p>In work on a recent project, the client developers were using WampServer, but upon the site reaching a certain size, developers on Windows noted that their Apache processes were quitting after a cache clear. We debugged and tracked the errors down to occuring during CSS preprocessing.  The Apache processes were segmentation faulting, resulting in an error dialog.</p></p>
<p>In an initial assessment of the problem, it appeared that others were having the same problem, such as in <a href="http://drupal.org/node/424136" title="http://drupal.org/node/424136">http://drupal.org/node/424136</a>. We advised the developers to add a line to their settings.php to disable css preprocessing, like so:</p></p>

<!--more-->

<p>
<div class="geshifilter">
<div class="php geshifilter-php"><span class="re0">$conf</span> <span class="sy0">=</span> <a href="http://www.php.net/array"><span class="kw3">array</span></a><span class="br0">(</span><br /><br />
  <span class="st_h">'preprocess_css'</span> <span class="sy0">=></span> <span class="kw4">FALSE</span><span class="sy0">,</span><br /><br />
<span class="br0">)</span><span class="sy0">;</span></div></div></p></p>
<p>This worked fine for awhile, but then the same problem came back in a different situation. Developers on Windows began to see the same segmentation faults on the main Features administration page or when reverting all Features. This led me to the real cause of these segmentation faults - stack overflows.  Both CSS preprocessing and Features reversion result in a number of recursive calls that will overflow a small stack, and therein lies a solution.</p></p>
<p>By default, Mac OS X allocates 8 megabytes of stack space for processes, while Windows allocates a much smaller amount (which looks to be closer to 1 megabyte.)  By changing the Apache configuration to allocate the same 8 megabytes of stack space per Apache thread, developers on Windows no longer experienced segmentation faults when optimizing CSS files or when reverting Features.</p></p>
<p>In order to set the stack size back up to 8 megabytes, you need to set the <code>ThreadStackSize</code> parameter in your Apache configuration files to <code>8388608</code>.  (The value is expressed in bytes.)</p></p>
<p>If this issue is affecting you on Windows, here are the two changes that you will need to make to your Windows configuration under WampServer.</p></p>
<p>In your main apache configuration file, uncomment the line that includes the httpd-mpm.conf file, like so:</p></p>
<p>
<div class="geshifilter">
<div class="php geshifilter-php"><span class="co2"># Server-pool management (MPM specific)<br /><br />
</span><span class="kw1">Include</span> conf<span class="sy0">/</span>extra<span class="sy0">/</span>httpd<span class="sy0">-</span>mpm<span class="sy0">.</span>conf</div></div></p></p>
<p>And then in the httpd-mpm.conf file, add a line to set the stack size up to 8 MB (as it is on Mac and Linux Apache by default.) Specifically, add the third line in:</p></p>
<p>
<div class="geshifilter">
<div class="php geshifilter-php"><span class="sy0"><</span>IfModule mpm_winnt_module<span class="sy0">></span><br /><br />
    ThreadsPerChild      <span class="nu0">150</span><br /><br />
    MaxRequestsPerChild    <span class="nu0">0</span><br /><br />
    ThreadStackSize  <span class="nu0">8388608</span><br /><br />
<span class="sy0"></</span>IfModule<span class="sy0">></span></div></div></p></p>
<p>Once Apache is restarted, you should be free of segmentation faults (or at least you shouldn't run into problems until your colleagues on Linux and Mac OS do.)</p></p>
