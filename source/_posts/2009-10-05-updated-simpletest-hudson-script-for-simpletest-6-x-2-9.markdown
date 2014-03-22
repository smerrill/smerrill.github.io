---
layout: post
status: publish
published: true
title: Updated SimpleTest Hudson Script for SimpleTest 6.x-2.9
author: Steven Merrill
author_login: smerrill
author_email: info@phase2technology.com
excerpt: <p>The fine folks at ComputerMinds recently posted <a href="http://www.computerminds.co.uk/hudson-and-simpletest">a
  modified SimpleTest run-tests.sh script</a> for running SimpleTests from the command
  line. &nbsp;Their script added an --xml option to allow the script to run tests
  and output results in JUnit's XML format so that Hudson can automatically run all
  SimpleTests in your project.</p>
wordpress_id: 1174
wordpress_url: http://phase2technology.com/updated-simpletest-hudson-script-for-simpletest-6-x-2-9/
date: !binary |-
  MjAwOS0xMC0wNSAxODowMzowNCAtMDQwMA==
date_gmt: !binary |-
  MjAwOS0xMC0wNSAyMzowMzowNCAtMDQwMA==
categories:
- Uncategorized
tags:
- continuous integration
- hudson
- pressflow
- simpletest
- unit testing
comments: []
---
<p>The fine folks at ComputerMinds recently posted <a href="http:&#47;&#47;www.computerminds.co.uk&#47;hudson-and-simpletest">a modified SimpleTest run-tests.sh script<&#47;a> for running SimpleTests from the command line. &nbsp;Their script added an --xml option to allow the script to run tests and output results in JUnit's XML format so that Hudson can automatically run all SimpleTests in your project.<&#47;p></p>
<p>SimpleTest is also included inside of Four Kitchens' <a href="http:&#47;&#47;fourkitchens.com&#47;pressflow-makes-drupal-scale">Pressflow distribution<&#47;a>. &nbsp;The latest version of SimpleTest is 6.x-2.9, which you will also get if you upgrade your version of Pressflow past bzr revision 49. &nbsp;Attached to this post is an upgraded version of the run-tests.sh script suitable for use with SimpleTest 6.x-2.9, regardless of if you are using Pressflow or not.<&#47;p></p>
<p>The relevant d.o issue is at&nbsp;http:&#47;&#47;drupal.org&#47;node&#47;602332, but <a href="https:&#47;&#47;code.launchpad.net&#47;~smerrill&#47;pressflow&#47;simpletest-xml-junit&#47;+merge&#47;13224">the active discussion<&#47;a> around this issue is already happening in <a href="https:&#47;&#47;launchpad.net&#47;pressflow">Pressflow's Launchpad project<&#47;a> where some of the team working on the Economist are going to compare notes shortly.<&#47;p></p>
<p>Enjoy!<&#47;p></p>
