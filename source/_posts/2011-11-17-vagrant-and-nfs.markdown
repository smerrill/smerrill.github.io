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
wordpress_url: http://phase2technology.com/vagrant-and-nfs/
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
<p>One of the most useful features of Vagrant is that it has the ability to share files with the VMs it manages, which lets your team work with the tools they're used to while still getting the benefits of running the full production stack.<&#47;p></p>
<p>It can share those files from the host (the machine running VirtualBox and Vagrant) to the guest (the virtualized Linux machine) via VirtualBox's built-in file sharing on Mac, Windows, or Linux. When run on Linux or Mac hosts, it can also share files to the guest via NFS.  NFS performs much better for sharing large numbers of files on a Linux or Mac host, which is <a href="http:&#47;&#47;vagrantup.com&#47;docs&#47;nfs.html">well documented in the excellent Vagrant documentation<&#47;a>. In addition, remember that the directory with the Vagrantfile in it will be shared with VirtualBox's built-in file sharing, so we probably don't want to put our docroot right in that directory.<&#47;p></p>
<p>As a result, we usually set up our project directories to look like the following with the docroot one level up from the directory with the Vagrantfile in it. (The docroot can be a symlink or a real copy of the docroot - it is in the .gitignore file so it won't be committed.)<&#47;p></p>
<p><img src="https:&#47;&#47;img.skitch.com&#47;20111117-j5tdm5q6pw584sfnnafxrtawb8.jpg" alt="Vagrant setup with the treehouseagency.com folder on the same level as the folder that contains the Vagrantfile" &#47;><&#47;p></p>
<p>There's a few gotchas with using NFS folders:<&#47;p></p>
<ul>
<li>If you try to export a symlink, <code>nfsd<&#47;code> will complain. You need to dereference any symlinks before they are put in &#47;etc&#47;exports. The sample code below avoids that.<&#47;li>
<li>Vagrant automatically uses NFS's mapall to ensure that all file access on the guest maps to your user and group on the host. This will mean that the users and groups may look wrong on the VM (and and <code>chown<&#47;code> or <code>chgrp<&#47;code> commands will fail), but software on the VM will be able to write to everything in that directory.<&#47;li><br />
<&#47;ul></p>
<p>Finally, here's some stub code for setting up a vagrant project with a docroot one level up. It takes care of dereferencing any symlinks and sharing the docroot with the guest. It will share it with NFS if the host is a Linux or Mac machine.<&#47;p></p>
<p>
<div class="geshifilter">
<div class="ruby geshifilter-ruby"><span class="co1"># Set up some variables relating to which path Vagrant will try to share<&#47;span><br &#47;><br />
<span class="co1"># with the VM.<&#47;span><br &#47;><br />
<span class="kw3">require<&#47;span> <span class="st0">'pathname'<&#47;span><br &#47;><br />
<span class="re0">$docroot_name<&#47;span> = <span class="st0">"treehouseagency.com"<&#47;span><br &#47;><br />
<span class="re0">$docroot_path<&#47;span> = <span class="st0">""<&#47;span><br &#47;><br />
<br &#47;><br />
<span class="co1"># Test that the directory to be shared is in the right place and if it is,<&#47;span><br &#47;><br />
<span class="co1"># calculate the fully dereferenced path (since NFS exports will fail if you<&#47;span><br &#47;><br />
<span class="co1"># try to specify the path to a symlink.)<&#47;span><br &#47;><br />
<span class="kw1">if<&#47;span> !<span class="kw4">File<&#47;span>.<span class="me1">exists<&#47;span>?<span class="br0">&#40;<&#47;span><span class="st0">"..&#47;#{$docroot_name}"<&#47;span><span class="br0">&#41;<&#47;span> <span class="kw1">then<&#47;span><br &#47;><br />
&nbsp; <span class="kw3">puts<&#47;span> <span class="st0">"Please put the '%s' directory (or a symlink to it) in the '%s&#47;' directory."<&#47;span> <span class="sy0">%<&#47;span><br &#47;><br />
&nbsp; &nbsp; <span class="br0">&#91;<&#47;span>$docroot_name, <span class="kw4">Pathname<&#47;span>.<span class="me1">new<&#47;span><span class="br0">&#40;<&#47;span><span class="st0">"..&#47;"<&#47;span><span class="br0">&#41;<&#47;span>.<span class="me1">realpath<&#47;span>.<span class="me1">to_s<&#47;span><span class="br0">&#93;<&#47;span><br &#47;><br />
&nbsp; <span class="kw3">exit<&#47;span> <span class="nu0">1<&#47;span><br &#47;><br />
<span class="kw1">else<&#47;span><br &#47;><br />
&nbsp; <span class="re0">$docroot_path<&#47;span> = <span class="kw4">Pathname<&#47;span>.<span class="me1">new<&#47;span><span class="br0">&#40;<&#47;span><span class="st0">"..&#47;#{$docroot_name}"<&#47;span><span class="br0">&#41;<&#47;span>.<span class="me1">realpath<&#47;span>.<span class="me1">to_s<&#47;span><br &#47;><br />
<span class="kw1">end<&#47;span><br &#47;><br />
<br &#47;><br />
<span class="re2">Vagrant::Config<&#47;span>.<span class="me1">run<&#47;span> <span class="kw1">do<&#47;span> <span class="sy0">|<&#47;span>config<span class="sy0">|<&#47;span><br &#47;><br />
<br &#47;><br />
&nbsp; <span class="co1"># Add in the rest of your config here.<&#47;span><br &#47;><br />
<br &#47;><br />
&nbsp; config.<span class="me1">vm<&#47;span>.<span class="me1">share_folder<&#47;span> <span class="st0">"tha-docroot"<&#47;span>, <span class="st0">"&#47;tha-docroot"<&#47;span>, <span class="re0">$docroot_path<&#47;span>, <span class="re3">:nfs<&#47;span> <span class="sy0">=><&#47;span> <span class="br0">&#40;<&#47;span>RUBY_PLATFORM =~ <span class="sy0">&#47;<&#47;span>linux<span class="sy0">&#47;<&#47;span> <span class="kw1">or<&#47;span> RUBY_PLATFORM =~ <span class="sy0">&#47;<&#47;span>darwin<span class="sy0">&#47;<&#47;span><span class="br0">&#41;<&#47;span><br &#47;><br />
<span class="kw1">end<&#47;span><&#47;div><&#47;div><&#47;p></p>
<p>Enjoy!<&#47;p></p>
