---
layout: post
status: publish
published: true
title: Vagrant and NFS
author: Steven Merrill
author_login: smerrill
author_email: "info@phase2technology.com"
excerpt: "<p>One of the most useful features of Vagrant is that it has the ability to share files with the VMs it manages, which lets your team work with the tools they're used to while still getting the benefits of running the full production stack.</p>"
wordpress_id: 1084
wordpress_url: "http://www.phase2technology.com/vagrant-and-nfs/"
categories: 
  - drupal planet
  - nfs
  - ops
  - ruby
  - vagrant
  - virtualization
comments: []
---

One of the most useful features of Vagrant is that it has the ability to share files with the VMs it manages, which lets your team work with the tools they're used to while still getting the benefits of running the full production stack.

It can share those files from the host (the machine running VirtualBox and Vagrant) to the guest (the virtualized Linux machine) via VirtualBox's built-in file sharing on Mac, Windows, or Linux. When run on Linux or Mac hosts, it can also share files to the guest via NFS.  NFS performs much better for sharing large numbers of files on a Linux or Mac host, which is <a href="http://vagrantup.com/docs/nfs.html">well documented in the excellent Vagrant documentation</a>. In addition, remember that the directory with the Vagrantfile in it will be shared with VirtualBox's built-in file sharing, so we probably don't want to put our docroot right in that directory.

As a result, we usually set up our project directories to look like the following with the docroot one level up from the directory with the Vagrantfile in it. (The docroot can be a symlink or a real copy of the docroot - it is in the .gitignore file so it won't be committed.)

<img src="https://img.skitch.com/20111117-j5tdm5q6pw584sfnnafxrtawb8.jpg" alt="Vagrant setup with the treehouseagency.com folder on the same level as the folder that contains the Vagrantfile" />

There's a few gotchas with using NFS folders:
- If you try to export a symlink, <code>nfsd</code> will complain. You need to dereference any symlinks before they are put in /etc/exports. The sample code below avoids that.</li>
- Vagrant automatically uses NFS's mapall to ensure that all file access on the guest maps to your user and group on the host. This will mean that the users and groups may look wrong on the VM (and and <code>chown</code> or <code>chgrp</code> commands will fail), but software on the VM will be able to write to everything in that directory.

Finally, here's some stub code for setting up a vagrant project with a docroot one level up. It takes care of dereferencing any symlinks and sharing the docroot with the guest. It will share it with NFS if the host is a Linux or Mac machine.

```ruby
# Set up some variables relating to which path Vagrant will try to share
# with the VM.
require 'pathname'
$docroot_name = "treehouseagency.com"
$docroot_path = ""
# Test that the directory to be shared is in the right place and if it is,
# calculate the fully dereferenced path (since NFS exports will fail if you
# try to specify the path to a symlink.)
if !File.exists?("../#{$docroot_name}") then
  puts "Please put the '%s' directory (or a symlink to it) in the '%s/' directory." %
    [$docroot_name, Pathname.new("../").realpath.to_s]
  exit 1
else
  $docroot_path = Pathname.new("../#{$docroot_name}").realpath.to_s
end
Vagrant::Config.run do |config|
  # Add in the rest of your config here.
  config.vm.share_folder "tha-docroot", "/tha-docroot", $docroot_path, :nfs => (RUBY_PLATFORM =~ /linux/ or RUBY_PLATFORM =~ /darwin/)
end
```

Enjoy!
