---
layout: post
status: publish
published: true
title: Using HipChat through an IRC client with BitlBee
author: Steven Merrill
author_login: smerrill
author_email: info@phase2technology.com
excerpt: '<p>Here at Treehouse Agency, we love IRC, as does the rest of the Drupal
  community. Still, IRC ports are often blocked, and not everyone is comfortable using
  IRC. We''ve recently been using <a href="http://www.hipchat.com/">HipChat</a> to
  set up chat rooms for certain new clients.</p>

  <p>I already route most of my AIM and GTalk interaction through an IRC gateway using
  <a href="http://www.bitlbee.org/">BitlBee</a>, and I wanted to hook HipChat up to
  an IRC client as well. Here''s a guide on how to do this.</p>'
wordpress_id: 1070
wordpress_url: http://www.phase2technology.com/using-hipchat-through-an-irc-client-with-bitlbee/
categories:
- development
- bitlbee
- hipchat
- irc
- jabber
- phase2blog
comments: []
---
<p>Here at Treehouse Agency, we love IRC, as does the rest of the Drupal community. Still, IRC ports are often blocked, and not everyone is comfortable using IRC. We've recently been using <a href="http://www.hipchat.com/">HipChat</a> to set up chat rooms for certain new clients.</p></p>
<p>I already route most of my AIM and GTalk interaction through an IRC gateway using <a href="http://www.bitlbee.org/">BitlBee</a>, and I wanted to hook HipChat up to an IRC client as well. Here's a guide on how to do this.</p></p>
<p>(Note that I was having trouble joining channels in my LimeChat last night as I was writing this up, but I might just be missing something. Try it out!)</p></p>

<!--more-->

<p>All the uppercase items come from the <a href="https://www.hipchat.com/account/xmpp">XMPP Account Settings page</a> in your HipChat account.</p></p>
<ol>
<li>Add the account in Bitlbee.
<ul>
<li><code>account add jabber USERNAME@chat.hipchat.com 'PASSWORD'</code></li><br />
</ul><br />
</li></p>
<li>Before you connect, set it up so that folks' chat names will display properly.
<ul>
<li><code>account hipchat set nick_source full_name</code></li><br />
</ul><br />
</li></p>
<li>To prevent being spammed with room history every time you join or change status, set your resource to 'bot'.
<ul>
<li><code>account hipchat set resource bot</code></li><br />
</ul><br />
</li></p>
<li>Connect to the account.
<ul>
<li><code>account hipchat on</code></li><br />
</ul><br />
</li></p>
<li>At this point, you can join the &hipchat channel to see all your contacts and use /msg to start talking w/ one of them privately.
<ul>
<li><code>/join &hipchat</code></li><br />
</ul><br />
</li></p>
<li>Now, add a chat. Note that the information on the XMPP settings pages doesn't give you @chat.hipchat.com. You can set whatever channel name you want.
<ul>
<li><code>chat add hipchat JABBERNAME@chat.hipchat.com #CHANNELNAME</code></li><br />
</ul><br />
</li></p>
<li>Before you can join, you must set your nick to match the <strong>Room nickname</strong> setting in your XMPP settings. You must do this for every room. Use the channel name you decided on earlier for easier access.
<ul>
<li><code>channel #CHANNELNAME set nick 'ROOM NICKNAME'</code></li><br />
</ul><br />
</li></p>
<li>Now, join the channel.
<ul>
<li><code>/join #CHANNELNAME</code></li><br />
</ul><br />
</li></p>
<li>Start chatting!
<ul>
<li><code>Hey everybody!</code></li>
<li><code>I'm using IRC!</code></li>
<li><code>(fry)</code></li>
<li><code>/me likes IRC and HipChat</code></li><br />
</ul><br />
</li></p>
<li>When you're ready to sign out of HipChat, use <code>account off</code>.
<ul>
<li><code>account hipchat off</code></li><br />
</ul><br />
</li><br />
</ol></p>
