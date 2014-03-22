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
date: !binary |-
  MjAxMC0xMS0xNiAxMjozNzozNyAtMDUwMA==
date_gmt: !binary |-
  MjAxMC0xMS0xNiAxNzozNzozNyAtMDUwMA==
categories:
- Uncategorized
tags:
- segmentation fault
- wamp
- windows
comments: []
---
<p>At Treehouse Agency, we often work with internal development teams, and enterprise software being what it is, they often run Windows. This has been the primary driver behind some of our technology choices (using Mercurial rather than Git on these sorts of projects) and it also occasionally necessitates some extra debugging when something doesn't quite work right on Windows.<&#47;p></p>
<p>In work on a recent project, the client developers were using WampServer, but upon the site reaching a certain size, developers on Windows noted that their Apache processes were quitting after a cache clear. We debugged and tracked the errors down to occuring during CSS preprocessing.  The Apache processes were segmentation faulting, resulting in an error dialog.<&#47;p></p>
<p>In an initial assessment of the problem, it appeared that others were having the same problem, such as in <a href="http:&#47;&#47;drupal.org&#47;node&#47;424136" title="http:&#47;&#47;drupal.org&#47;node&#47;424136">http:&#47;&#47;drupal.org&#47;node&#47;424136<&#47;a>. We advised the developers to add a line to their settings.php to disable css preprocessing, like so:<&#47;p></p>
<p>
<div class="geshifilter">
<div class="php geshifilter-php"><span class="re0">$conf<&#47;span> <span class="sy0">=<&#47;span> <a href="http:&#47;&#47;www.php.net&#47;array"><span class="kw3">array<&#47;span><&#47;a><span class="br0">&#40;<&#47;span><br &#47;><br />
&nbsp; <span class="st_h">'preprocess_css'<&#47;span> <span class="sy0">=><&#47;span> <span class="kw4">FALSE<&#47;span><span class="sy0">,<&#47;span><br &#47;><br />
<span class="br0">&#41;<&#47;span><span class="sy0">;<&#47;span><&#47;div><&#47;div><&#47;p></p>
<p>This worked fine for awhile, but then the same problem came back in a different situation. Developers on Windows began to see the same segmentation faults on the main Features administration page or when reverting all Features. This led me to the real cause of these segmentation faults - stack overflows.  Both CSS preprocessing and Features reversion result in a number of recursive calls that will overflow a small stack, and therein lies a solution.<&#47;p></p>
<p>By default, Mac OS X allocates 8 megabytes of stack space for processes, while Windows allocates a much smaller amount (which looks to be closer to 1 megabyte.)  By changing the Apache configuration to allocate the same 8 megabytes of stack space per Apache thread, developers on Windows no longer experienced segmentation faults when optimizing CSS files or when reverting Features.<&#47;p></p>
<p>In order to set the stack size back up to 8 megabytes, you need to set the <code>ThreadStackSize<&#47;code> parameter in your Apache configuration files to <code>8388608<&#47;code>.  (The value is expressed in bytes.)<&#47;p></p>
<p>If this issue is affecting you on Windows, here are the two changes that you will need to make to your Windows configuration under WampServer.<&#47;p></p>
<p>In your main apache configuration file, uncomment the line that includes the httpd-mpm.conf file, like so:<&#47;p></p>
<p>
<div class="geshifilter">
<div class="php geshifilter-php"><span class="co2"># Server-pool management (MPM specific)<br &#47;><br />
<&#47;span><span class="kw1">Include<&#47;span> conf<span class="sy0">&#47;<&#47;span>extra<span class="sy0">&#47;<&#47;span>httpd<span class="sy0">-<&#47;span>mpm<span class="sy0">.<&#47;span>conf<&#47;div><&#47;div><&#47;p></p>
<p>And then in the httpd-mpm.conf file, add a line to set the stack size up to 8 MB (as it is on Mac and Linux Apache by default.) Specifically, add the third line in:<&#47;p></p>
<p>
<div class="geshifilter">
<div class="php geshifilter-php"><span class="sy0"><<&#47;span>IfModule mpm_winnt_module<span class="sy0">><&#47;span><br &#47;><br />
&nbsp; &nbsp; ThreadsPerChild &nbsp; &nbsp; &nbsp;<span class="nu0">150<&#47;span><br &#47;><br />
&nbsp; &nbsp; MaxRequestsPerChild &nbsp; &nbsp;<span class="nu0">0<&#47;span><br &#47;><br />
&nbsp; &nbsp; ThreadStackSize &nbsp;<span class="nu0">8388608<&#47;span><br &#47;><br />
<span class="sy0"><&#47;<&#47;span>IfModule<span class="sy0">><&#47;span><&#47;div><&#47;div><&#47;p></p>
<p>Once Apache is restarted, you should be free of segmentation faults (or at least you shouldn't run into problems until your colleagues on Linux and Mac OS do.)<&#47;p></p>
