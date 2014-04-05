---
layout: post
status: publish
published: true
title: "Ensuring your Vagrant's box is weatherproof: A quick Veewee tip"
author: Steven Merrill
author_login: smerrill
author_email: "info@phase2technology.com"
excerpt: "<p>We'll be doing a screencast series soon on using the <a href=\"http://www.vagrantup.com/\">Vagrant gem</a> to distribute and manage virtual machines so that your entire team (yes, even Windows folks!) can do development on their local machine with the same software that's on your production Linux servers.</p>"
wordpress_id: 1110
wordpress_url: "http://www.phase2technology.com/ensuring-your-vagrants-box-is-weatherproof-a-quick-veewee-tip/"
categories: 
  - development
  - vagrant
  - veewee
comments: []
---

We'll be doing a screencast series soon on using the <a href="http://www.vagrantup.com/">Vagrant gem</a> to distribute and manage virtual machines so that your entire team (yes, even Windows folks!) can do development on their local machine with the same software that's on your production Linux servers.

Another useful tool in the Vagrant user's arsenal is <a href="https://github.com/jedi4ever/veewee">Veewee</a>. Veewee lets you automate the VirtualBox application to install a full operating system with just the packages you want and need. Veewee does have some built-in validation tools, such as <code>vagrant basebox validate BOXNAME</code>, which will run a set of Cucumber acceptance tests to ensure that the virtual machine should work properly when brought up with Vagrant, as well as with the <a href="http://www.opscode.com/chef/">Chef</a> and <a href="http://puppetlabs.com/">Puppet</a> configuration management tools.

Nonetheless, sometimes you might want to quickly pop onto the VM that's just been built by veewee before issuing a <code>vagrant basebox export BOXNAME</code> to save it to a .box file suitable for distribution. To easily enable this, just add the following to your <code>~/.ssh/config</code> file.

{% highlight text %}
# Veewee box.
Host veewee-machine
  HostName 127.0.0.1
  User vagrant
  Port 7222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /home/smerrill/.rvm/gems/ruby-1.9.2-p290/gems/vagrant-0.8.6/keys/vagrant
  IdentitiesOnly yes
{% endhighlight %}

You'll need to change the <code>IdentityFile</code> directive to point at wherever Vagrant is installed on your machine. With the entry in your SSH config file in place, you can simply execute an <code>ssh veewee-machine</code> and kick the tires before bundling up the box.

Now go forth and build an army of virtual machines!