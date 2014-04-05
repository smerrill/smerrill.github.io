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
wordpress_url: http://www.phase2technology.com/ensuring-your-vagrants-box-is-weatherproof-a-quick-veewee-tip/
categories:
- development
- vagrant
- veewee
comments: []
---
<p>We'll be doing a screencast series soon on using the <a href="http://www.vagrantup.com/">Vagrant gem</a> to distribute and manage virtual machines so that your entire team (yes, even Windows folks!) can do development on their local machine with the same software that's on your production Linux servers.</p></p>
<p>Another useful tool in the Vagrant user's arsenal is <a href="https://github.com/jedi4ever/veewee">Veewee</a>. Veewee lets you automate the VirtualBox application to install a full operating system with just the packages you want and need. Veewee does have some built-in validation tools, such as <code>vagrant basebox validate BOXNAME</code>, which will run a set of Cucumber acceptance tests to ensure that the virtual machine should work properly when brought up with Vagrant, as well as with the <a href="http://www.opscode.com/chef/">Chef</a> and <a href="http://puppetlabs.com/">Puppet</a> configuration management tools.</p></p>
<p>Nonetheless, sometimes you might want to quickly pop onto the VM that's just been built by veewee before issuing a <code>vagrant basebox export BOXNAME</code> to save it to a .box file suitable for distribution. To easily enable this, just add the following to your <code>~/.ssh/config</code> file.</p></p>
<p>
<div class="geshifilter">
<div class="php geshifilter-php"><span class="co2"># Veewee box.<br /><br />
</span>Host veewee<span class="sy0">-</span>machine<br /><br />
  HostName 127<span class="sy0">.</span>0<span class="sy0">.</span>0<span class="sy0">.</span>1<br /><br />
  User vagrant<br /><br />
  Port <span class="nu0">7222</span><br /><br />
  UserKnownHostsFile <span class="sy0">/</span>dev<span class="sy0">/</span><span class="kw4">null</span><br /><br />
  StrictHostKeyChecking no<br /><br />
  PasswordAuthentication no<br /><br />
  IdentityFile <span class="sy0">/</span>home<span class="sy0">/</span>smerrill<span class="sy0">/.</span>rvm<span class="sy0">/</span>gems<span class="sy0">/</span>ruby<span class="sy0">-</span>1<span class="sy0">.</span>9<span class="sy0">.</span>2<span class="sy0">-</span>p290<span class="sy0">/</span>gems<span class="sy0">/</span>vagrant<span class="sy0">-</span>0<span class="sy0">.</span>8<span class="sy0">.</span>6<span class="sy0">/</span>keys<span class="sy0">/</span>vagrant<br /><br />
  IdentitiesOnly yes</div></div></p></p>
<p>You'll need to change the <code>IdentityFile</code> directive to point at wherever Vagrant is installed on your machine. With the entry in your SSH config file in place, you can simply execute an <code>ssh veewee-machine</code> and kick the tires before bundling up the box.</p></p>
<p>Now go forth and build an army of virtual machines!</p></p>
