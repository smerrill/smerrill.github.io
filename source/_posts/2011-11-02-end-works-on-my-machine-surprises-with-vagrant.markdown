---
layout: post
status: publish
published: true
title: End "Works on My Machine" Surprises with Vagrant
author: Steven Merrill
author_login: smerrill
author_email: info@phase2technology.com
excerpt: '<p>How many times have the following issues happened on a project you''ve
  worked on?</p>

  <ul>

  <li>Notices (or worse) appeared on production because of a PHP version mismatch
  between a developer''s machine and the production web servers.</li>

  <li>A new PHP extension or PECL extension had to be installed on production because
  it was installed in WAMP or MAMP?</li>

  <li>A team member ran into difficult setting up their local environment and spent
  many hours stuck on something.</li>

  <li>Team members didn''t set up SSL or Varnish on their local machines and issues
  had to be caught on a dev server.</li>

  </ul>'
wordpress_id: 1108
wordpress_url: http://www.phase2technology.com/end-works-on-my-machine-surprises-with-vagrant/
categories:
- chef
- drupal planet
- hosting
- planet drupal
- puppet
- vagrant
- veewee
- virtualization
- phase2blog
comments: []
---
<p>How many times have the following issues happened on a project you've worked on?</p></p>
<ul>
<li>Notices (or worse) appeared on production because of a PHP version mismatch between a developer's machine and the production web servers.</li>
<li>A new PHP extension or PECL extension had to be installed on production because it was installed in WAMP or MAMP?</li>
<li>A team member ran into difficult setting up their local environment and spent many hours stuck on something.</li>
<li>Team members didn't set up SSL or Varnish on their local machines and issues had to be caught on a dev server.</li>
<li>A team member would like to switch to Homebrew, but can't set aside the many hours to redo their setup until a project is done.</li><br />
</ul></p>

<!--more-->

<p>Tools like <a href="http://www.mamp.info/en/index.html">MAMP</a>, <a href="http://www.apachefriends.org/en/xampp.html">XAMPP</a>, the <a href="http://network.acquia.com/downloads">Aqcuia dev desktop</a>, <a href="http://www.macports.org/">MacPorts</a> and <a href="http://mxcl.github.com/homebrew/">Homebrew</a> all make it easy to get an *AMP stack up and running on your computer, and tools like MacPorts and Homebrew even make it pretty easy to install tools like <a href="https://www.varnish-cache.org/">Varnish</a> and <a href="http://memcached.org/">memcached</a>.</p></p>
<p>While these tools make it easy to run a very close approximation of the production hosting stack on your local machine (arguably closer if you use Macintosh or Linux,) it will still have some key differences which will ultimately contribute at some point to a "<a href="http://www.codinghorror.com/blog/2007/03/the-works-on-my-machine-certification-program.html">Works on My Machine!</a>" situation in your project.</p></p>
<p><img alt="Works On My Machine Badge" src="http://treehouseagency.com/sites/treehouseagency.com/files/worksonmymachine_0.png" style="border: medium none; display: block; float: left;" /></p></p>
<p>Luckily, virtualization has advanced to such a degree that there are cross-platform virtualization solutions such as <a href="https://www.virtualbox.org/">VirtualBox</a>, but just using a VM inside of VirtualBox doesn't solve the whole problem. It makes acquiring the correct versions of software easy, but keeping configuration in sync can still be a challenge for users who are not deeply familiar with Linux.</p></p>
<p>Enter <a href="http://vagrantup.com/">Vagrant</a>.</p></p>
<p>Vagrant is a Ruby gem that makes working with Linux virtual machines easy. You distribute a Vagrantfile to your team, and it does the following things for you:</p></p>
<ul>
<li>Downloads and sets up virtual machines from a single .box file which it will download over HTTP or FTP.</li>
<li>Provisions the software and configuration on the VM using your choice of Chef, Puppet, or simple shell scripts</li>
<li>Automatically shares the directory with the Vagrantfile (and any subdirectories) to the virtual machine with Virtualbox's built-in file sharing</li>
<li>Forwards the SSH port (and optionally other ports) to your localhost and avoids collisions so you can always directly SSH to the machine</li>
<li>Optionally sets up a dedicated host-only IP address that you can use to connect to all services on the VM without port forwarding</li>
<li>Optionally shares directories to the VM over NFS from a Macintosh or Linux guest, which enables acceptable performance for a Drupal docroot</li><br />
</ul></p>
<p>Since Vagrant handles the file sharing with the VM, you and your team don't have to mess around with setting up FUSE or the like and you can continue to use the tools that you're used to using locally, such as your text editor or garphical source control program.</p></p>
<p>In addition, so long as you have a single developer skilled in ops who can encapsulate the production configuration into a system like Chef or Puppet, these changes can be pushed down to the whole team. Once your ops team has a working Varnish configuration, for example, they can push that into the Vagrant repository, and then a working Varnish setup on all your developers' VMs is just a <code>git pull</code> and a <code>vagrant provision</code> away.</p></p>
<p>We've been working with Vagrant over the last few months and think it offers a number of advantages. All it takes to get started installing VirtualBox and the <strong>vagrant</strong> ruby gem. Detailed information on how to get started is available in the excellent <a href="http://vagrantup.com/docs/getting-started/index.html">Vagrant Quickstart guide</a>.</p></p>
<p>I've put together a screencast that's just over 10 minutes long and shows the whole process of bringing up a CentOS 5.6 VM with the <a href="http://treehouseagency.com">treehouseagency.com</a> site shared from my local machine.</p></p>
<p><iframe src="http://player.vimeo.com/video/31494273?title=0&byline=0&portrait=0&color=ff9933" width="595" height="446" frameborder="0" webkitAllowFullScreen allowFullScreen></iframe></p></p>
<p>We'll be posting more example code over the coming weeks that will allow you to try out Drupal from your local machine on a Linux VM.</p></p>
