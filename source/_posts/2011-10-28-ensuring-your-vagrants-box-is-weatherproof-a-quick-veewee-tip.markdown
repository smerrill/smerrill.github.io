---
layout: post
status: publish
published: true
title: ! 'Ensuring your Vagrant&#039;s box is weatherproof: A quick Veewee tip'
author: Steven Merrill
author_login: smerrill
author_email: info@phase2technology.com
excerpt: <p>We'll be doing a screencast series soon on using the <a href="http://www.vagrantup.com/">Vagrant
  gem</a> to distribute and manage virtual machines so that your entire team (yes,
  even Windows folks!) can do development on their local machine with the same software
  that's on your production Linux servers.</p>
wordpress_id: 1110
wordpress_url: http://phase2technology.com/ensuring-your-vagrants-box-is-weatherproof-a-quick-veewee-tip/
date: !binary |-
  MjAxMS0xMC0yOCAxNDoxODoxMCAtMDQwMA==
date_gmt: !binary |-
  MjAxMS0xMC0yOCAxOToxODoxMCAtMDQwMA==
categories:
- Development
tags:
- vagrant
- veewee
comments: []
---
<p>We'll be doing a screencast series soon on using the <a href="http:&#47;&#47;www.vagrantup.com&#47;">Vagrant gem<&#47;a> to distribute and manage virtual machines so that your entire team (yes, even Windows folks!) can do development on their local machine with the same software that's on your production Linux servers.<&#47;p></p>
<p>Another useful tool in the Vagrant user's arsenal is <a href="https:&#47;&#47;github.com&#47;jedi4ever&#47;veewee">Veewee<&#47;a>. Veewee lets you automate the VirtualBox application to install a full operating system with just the packages you want and need. Veewee does have some built-in validation tools, such as <code>vagrant basebox validate BOXNAME<&#47;code>, which will run a set of Cucumber acceptance tests to ensure that the virtual machine should work properly when brought up with Vagrant, as well as with the <a href="http:&#47;&#47;www.opscode.com&#47;chef&#47;">Chef<&#47;a> and <a href="http:&#47;&#47;puppetlabs.com&#47;">Puppet<&#47;a> configuration management tools.<&#47;p></p>
<p>Nonetheless, sometimes you might want to quickly pop onto the VM that's just been built by veewee before issuing a <code>vagrant basebox export BOXNAME<&#47;code> to save it to a .box file suitable for distribution. To easily enable this, just add the following to your <code>~&#47;.ssh&#47;config<&#47;code> file.<&#47;p></p>
<p>
<div class="geshifilter">
<div class="php geshifilter-php"><span class="co2"># Veewee box.<br &#47;><br />
<&#47;span>Host veewee<span class="sy0">-<&#47;span>machine<br &#47;><br />
&nbsp; HostName 127<span class="sy0">.<&#47;span>0<span class="sy0">.<&#47;span>0<span class="sy0">.<&#47;span>1<br &#47;><br />
&nbsp; User vagrant<br &#47;><br />
&nbsp; Port <span class="nu0">7222<&#47;span><br &#47;><br />
&nbsp; UserKnownHostsFile <span class="sy0">&#47;<&#47;span>dev<span class="sy0">&#47;<&#47;span><span class="kw4">null<&#47;span><br &#47;><br />
&nbsp; StrictHostKeyChecking no<br &#47;><br />
&nbsp; PasswordAuthentication no<br &#47;><br />
&nbsp; IdentityFile <span class="sy0">&#47;<&#47;span>home<span class="sy0">&#47;<&#47;span>smerrill<span class="sy0">&#47;.<&#47;span>rvm<span class="sy0">&#47;<&#47;span>gems<span class="sy0">&#47;<&#47;span>ruby<span class="sy0">-<&#47;span>1<span class="sy0">.<&#47;span>9<span class="sy0">.<&#47;span>2<span class="sy0">-<&#47;span>p290<span class="sy0">&#47;<&#47;span>gems<span class="sy0">&#47;<&#47;span>vagrant<span class="sy0">-<&#47;span>0<span class="sy0">.<&#47;span>8<span class="sy0">.<&#47;span>6<span class="sy0">&#47;<&#47;span>keys<span class="sy0">&#47;<&#47;span>vagrant<br &#47;><br />
&nbsp; IdentitiesOnly yes<&#47;div><&#47;div><&#47;p></p>
<p>You'll need to change the <code>IdentityFile<&#47;code> directive to point at wherever Vagrant is installed on your machine. With the entry in your SSH config file in place, you can simply execute an <code>ssh veewee-machine<&#47;code> and kick the tires before bundling up the box.<&#47;p></p>
<p>Now go forth and build an army of virtual machines!<&#47;p></p>
