---
layout: post
status: publish
published: true
title: End "Works on My Machine" Surprises with Vagrant
author: Steven Merrill
author_login: smerrill
author_email: info@phase2technology.com
excerpt: ! '<p>How many times have the following issues happened on a project you''ve
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
date: !binary |-
  MjAxMS0xMS0wMiAxMToxOToxMSAtMDQwMA==
date_gmt: !binary |-
  MjAxMS0xMS0wMiAxNjoxOToxMSAtMDQwMA==
categories:
- Development
tags:
- chef
- drupal planet
- hosting
- planet drupal
- puppet
- vagrant
- veewee
- virtualization
comments: []
---
<p>How many times have the following issues happened on a project you've worked on?<&#47;p></p>
<ul>
<li>Notices (or worse) appeared on production because of a PHP version mismatch between a developer's machine and the production web servers.<&#47;li>
<li>A new PHP extension or PECL extension had to be installed on production because it was installed in WAMP or MAMP?<&#47;li>
<li>A team member ran into difficult setting up their local environment and spent many hours stuck on something.<&#47;li>
<li>Team members didn't set up SSL or Varnish on their local machines and issues had to be caught on a dev server.<&#47;li>
<li>A team member would like to switch to Homebrew, but can't set aside the many hours to redo their setup until a project is done.<&#47;li><br />
<&#47;ul></p>
<p>Tools like <a href="http:&#47;&#47;www.mamp.info&#47;en&#47;index.html">MAMP<&#47;a>, <a href="http:&#47;&#47;www.apachefriends.org&#47;en&#47;xampp.html">XAMPP<&#47;a>, the <a href="http:&#47;&#47;network.acquia.com&#47;downloads">Aqcuia dev desktop<&#47;a>, <a href="http:&#47;&#47;www.macports.org&#47;">MacPorts<&#47;a> and <a href="http:&#47;&#47;mxcl.github.com&#47;homebrew&#47;">Homebrew<&#47;a> all make it easy to get an &#42;AMP stack up and running on your computer, and tools like MacPorts and Homebrew even make it pretty easy to install tools like <a href="https:&#47;&#47;www.varnish-cache.org&#47;">Varnish<&#47;a> and <a href="http:&#47;&#47;memcached.org&#47;">memcached<&#47;a>.<&#47;p></p>
<p>While these tools make it easy to run a very close approximation of the production hosting stack on your local machine (arguably closer if you use Macintosh or Linux,) it will still have some key differences which will ultimately contribute at some point to a "<a href="http:&#47;&#47;www.codinghorror.com&#47;blog&#47;2007&#47;03&#47;the-works-on-my-machine-certification-program.html">Works on My Machine!<&#47;a>" situation in your project.<&#47;p></p>
<p><img alt="Works On My Machine Badge" src="http:&#47;&#47;treehouseagency.com&#47;sites&#47;treehouseagency.com&#47;files&#47;worksonmymachine_0.png" style="border: medium none; display: block; float: left;" &#47;><&#47;p></p>
<p>Luckily, virtualization has advanced to such a degree that there are cross-platform virtualization solutions such as <a href="https:&#47;&#47;www.virtualbox.org&#47;">VirtualBox<&#47;a>, but just using a VM inside of VirtualBox doesn't solve the whole problem. It makes acquiring the correct versions of software easy, but keeping configuration in sync can still be a challenge for users who are not deeply familiar with Linux.<&#47;p></p>
<p>Enter <a href="http:&#47;&#47;vagrantup.com&#47;">Vagrant<&#47;a>.<&#47;p></p>
<p>Vagrant is a Ruby gem that makes working with Linux virtual machines easy. You distribute a Vagrantfile to your team, and it does the following things for you:<&#47;p></p>
<ul>
<li>Downloads and sets up virtual machines from a single .box file which it will download over HTTP or FTP.<&#47;li>
<li>Provisions the software and configuration on the VM using your choice of Chef, Puppet, or simple shell scripts<&#47;li>
<li>Automatically shares the directory with the Vagrantfile (and any subdirectories) to the virtual machine with Virtualbox's built-in file sharing<&#47;li>
<li>Forwards the SSH port (and optionally other ports) to your localhost and avoids collisions so you can always directly SSH to the machine<&#47;li>
<li>Optionally sets up a dedicated host-only IP address that you can use to connect to all services on the VM without port forwarding<&#47;li>
<li>Optionally shares directories to the VM over NFS from a Macintosh or Linux guest, which enables acceptable performance for a Drupal docroot<&#47;li><br />
<&#47;ul></p>
<p>Since Vagrant handles the file sharing with the VM, you and your team don't have to mess around with setting up FUSE or the like and you can continue to use the tools that you're used to using locally, such as your text editor or garphical source control program.<&#47;p></p>
<p>In addition, so long as you have a single developer skilled in ops who can encapsulate the production configuration into a system like Chef or Puppet, these changes can be pushed down to the whole team. Once your ops team has a working Varnish configuration, for example, they can push that into the Vagrant repository, and then a working Varnish setup on all your developers' VMs is just a <code>git pull<&#47;code> and a <code>vagrant provision<&#47;code> away.<&#47;p></p>
<p>We've been working with Vagrant over the last few months and think it offers a number of advantages. All it takes to get started installing VirtualBox and the <strong>vagrant<&#47;strong> ruby gem. Detailed information on how to get started is available in the excellent <a href="http:&#47;&#47;vagrantup.com&#47;docs&#47;getting-started&#47;index.html">Vagrant Quickstart guide<&#47;a>.<&#47;p></p>
<p>I've put together a screencast that's just over 10 minutes long and shows the whole process of bringing up a CentOS 5.6 VM with the <a href="http:&#47;&#47;treehouseagency.com">treehouseagency.com<&#47;a> site shared from my local machine.<&#47;p></p>
<p><iframe src="http:&#47;&#47;player.vimeo.com&#47;video&#47;31494273?title=0&amp;byline=0&amp;portrait=0&amp;color=ff9933" width="595" height="446" frameborder="0" webkitAllowFullScreen allowFullScreen><&#47;iframe><&#47;p></p>
<p>We'll be posting more example code over the coming weeks that will allow you to try out Drupal from your local machine on a Linux VM.<&#47;p></p>
