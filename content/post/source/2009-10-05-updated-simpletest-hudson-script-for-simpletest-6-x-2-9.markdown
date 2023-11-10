---
author: Steven Merrill
author_email: info@phase2technology.com
author_login: smerrill
categories:
- continuous integration
- hudson
- pressflow
- simpletest
- unit testing
- phase2blog
comments: []
date: "2009-10-05T00:00:00Z"
excerpt: <p>The fine folks at ComputerMinds recently posted <a href="http://www.computerminds.co.uk/hudson-and-simpletest">a
  modified SimpleTest run-tests.sh script</a> for running SimpleTests from the command
  line.  Their script added an --xml option to allow the script to run tests and output
  results in JUnit's XML format so that Hudson can automatically run all SimpleTests
  in your project.</p>
published: true
status: publish
title: Updated SimpleTest Hudson Script for SimpleTest 6.x-2.9
wordpress_id: 1174
wordpress_url: http://www.phase2technology.com/updated-simpletest-hudson-script-for-simpletest-6-x-2-9/
---
<p>The fine folks at ComputerMinds recently posted <a href="http://www.computerminds.co.uk/hudson-and-simpletest">a modified SimpleTest run-tests.sh script</a> for running SimpleTests from the command line.  Their script added an --xml option to allow the script to run tests and output results in JUnit's XML format so that Hudson can automatically run all SimpleTests in your project.</p></p>

<!--more-->

<p>SimpleTest is also included inside of Four Kitchens' <a href="http://fourkitchens.com/pressflow-makes-drupal-scale">Pressflow distribution</a>.  The latest version of SimpleTest is 6.x-2.9, which you will also get if you upgrade your version of Pressflow past bzr revision 49.  Attached to this post is an upgraded version of the run-tests.sh script suitable for use with SimpleTest 6.x-2.9, regardless of if you are using Pressflow or not.</p></p>
<p>The relevant d.o issue is at http://drupal.org/node/602332, but <a href="https://code.launchpad.net/~smerrill/pressflow/simpletest-xml-junit/+merge/13224">the active discussion</a> around this issue is already happening in <a href="https://launchpad.net/pressflow">Pressflow's Launchpad project</a> where some of the team working on the Economist are going to compare notes shortly.</p></p>
<p>Enjoy!</p></p>
