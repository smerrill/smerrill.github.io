---
layout: post
status: publish
published: true
title: Vagrant and NFS
author: Steven Merrill
author_login: smerrill
author_email: info@phase2technology.com
excerpt: <p>One of the most useful features of Vagrant is that it has the ability
  to share files with the VMs it manages, which lets your team work with the tools
  they're used to while still getting the benefits of running the full production
  stack.</p>
wordpress_id: 1084
wordpress_url: http://www.phase2technology.com/vagrant-and-nfs/
date: !binary |-
  MjAxMS0xMS0xNyAxMTowMDo1MyAtMDUwMA==
date_gmt: !binary |-
  MjAxMS0xMS0xNyAxNjowMDo1MyAtMDUwMA==
categories:
- Development
tags:
- drupal planet
- nfs
- ops
- ruby
- vagrant
- virtualization
comments: []
---
<p>One of the most useful features of Vagrant is that it has the ability to share files with the VMs it manages, which lets your team work with the tools they're used to while still getting the benefits of running the full production stack.</p></p>
<p>It can share those files from the host (the machine running VirtualBox and Vagrant) to the guest (the virtualized Linux machine) via VirtualBox's built-in file sharing on Mac, Windows, or Linux. When run on Linux or Mac hosts, it can also share files to the guest via NFS.  NFS performs much better for sharing large numbers of files on a Linux or Mac host, which is <a href="http://vagrantup.com/docs/nfs.html">well documented in the excellent Vagrant documentation</a>. In addition, remember that the directory with the Vagrantfile in it will be shared with VirtualBox's built-in file sharing, so we probably don't want to put our docroot right in that directory.</p></p>
<p>As a result, we usually set up our project directories to look like the following with the docroot one level up from the directory with the Vagrantfile in it. (The docroot can be a symlink or a real copy of the docroot - it is in the .gitignore file so it won't be committed.)</p></p>
<p><img src="https://img.skitch.com/20111117-j5tdm5q6pw584sfnnafxrtawb8.jpg" alt="Vagrant setup with the treehouseagency.com folder on the same level as the folder that contains the Vagrantfile" /></p></p>
<p>There's a few gotchas with using NFS folders:</p></p>
<ul>
<li>If you try to export a symlink, <code>nfsd</code> will complain. You need to dereference any symlinks before they are put in /etc/exports. The sample code below avoids that.</li>
<li>Vagrant automatically uses NFS's mapall to ensure that all file access on the guest maps to your user and group on the host. This will mean that the users and groups may look wrong on the VM (and and <code>chown</code> or <code>chgrp</code> commands will fail), but software on the VM will be able to write to everything in that directory.</li><br />
</ul></p>
<p>Finally, here's some stub code for setting up a vagrant project with a docroot one level up. It takes care of dereferencing any symlinks and sharing the docroot with the guest. It will share it with NFS if the host is a Linux or Mac machine.</p></p>
<p>
<div class="geshifilter">
<div class="ruby geshifilter-ruby"><span class="co1"># Set up some variables relating to which path Vagrant will try to share</span><br /><br />
<span class="co1"># with the VM.</span><br /><br />
<span class="kw3">require</span> <span class="st0">'pathname'</span><br /><br />
<span class="re0">$docroot_name</span> = <span class="st0">"treehouseagency.com"</span><br /><br />
<span class="re0">$docroot_path</span> = <span class="st0">""</span><br /><br />
<br /><br />
<span class="co1"># Test that the directory to be shared is in the right place and if it is,</span><br /><br />
<span class="co1"># calculate the fully dereferenced path (since NFS exports will fail if you</span><br /><br />
<span class="co1"># try to specify the path to a symlink.)</span><br /><br />
<span class="kw1">if</span> !<span class="kw4">File</span>.<span class="me1">exists</span>?<span class="br0">(</span><span class="st0">"../#{$docroot_name}"</span><span class="br0">)</span> <span class="kw1">then</span><br /><br />
  <span class="kw3">puts</span> <span class="st0">"Please put the '%s' directory (or a symlink to it) in the '%s/' directory."</span> <span class="sy0">%</span><br /><br />
    <span class="br0">[</span>$docroot_name, <span class="kw4">Pathname</span>.<span class="me1">new</span><span class="br0">(</span><span class="st0">"../"</span><span class="br0">)</span>.<span class="me1">realpath</span>.<span class="me1">to_s</span><span class="br0">]</span><br /><br />
  <span class="kw3">exit</span> <span class="nu0">1</span><br /><br />
<span class="kw1">else</span><br /><br />
  <span class="re0">$docroot_path</span> = <span class="kw4">Pathname</span>.<span class="me1">new</span><span class="br0">(</span><span class="st0">"../#{$docroot_name}"</span><span class="br0">)</span>.<span class="me1">realpath</span>.<span class="me1">to_s</span><br /><br />
<span class="kw1">end</span><br /><br />
<br /><br />
<span class="re2">Vagrant::Config</span>.<span class="me1">run</span> <span class="kw1">do</span> <span class="sy0">|</span>config<span class="sy0">|</span><br /><br />
<br /><br />
  <span class="co1"># Add in the rest of your config here.</span><br /><br />
<br /><br />
  config.<span class="me1">vm</span>.<span class="me1">share_folder</span> <span class="st0">"tha-docroot"</span>, <span class="st0">"/tha-docroot"</span>, <span class="re0">$docroot_path</span>, <span class="re3">:nfs</span> <span class="sy0">=></span> <span class="br0">(</span>RUBY_PLATFORM =~ <span class="sy0">/</span>linux<span class="sy0">/</span> <span class="kw1">or</span> RUBY_PLATFORM =~ <span class="sy0">/</span>darwin<span class="sy0">/</span><span class="br0">)</span><br /><br />
<span class="kw1">end</span></div></div></p></p>
<p>Enjoy!</p></p>
