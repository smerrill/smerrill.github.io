---
layout: post
status: publish
published: true
title: Using HipChat through an IRC client with BitlBee
author: Steven Merrill
author_login: smerrill
author_email: info@phase2technology.com
excerpt: ! '<p>Here at Treehouse Agency, we love IRC, as does the rest of the Drupal
  community. Still, IRC ports are often blocked, and not everyone is comfortable using
  IRC. We''ve recently been using <a href="http://www.hipchat.com/">HipChat</a> to
  set up chat rooms for certain new clients.</p>

  <p>I already route most of my AIM and GTalk interaction through an IRC gateway using
  <a href="http://www.bitlbee.org/">BitlBee</a>, and I wanted to hook HipChat up to
  an IRC client as well. Here''s a guide on how to do this.</p>'
wordpress_id: 1070
wordpress_url: http://www.phase2technology.com/using-hipchat-through-an-irc-client-with-bitlbee/
date: !binary |-
  MjAxMS0xMi0xNSAxMDoxOTo1MiAtMDUwMA==
date_gmt: !binary |-
  MjAxMS0xMi0xNSAxNToxOTo1MiAtMDUwMA==
categories:
- Development
tags:
- bitlbee
- hipchat
- irc
- jabber
comments: []
---
<p>Here at Treehouse Agency, we love IRC, as does the rest of the Drupal community. Still, IRC ports are often blocked, and not everyone is comfortable using IRC. We've recently been using <a href="http:&#47;&#47;www.hipchat.com&#47;">HipChat<&#47;a> to set up chat rooms for certain new clients.<&#47;p></p>
<p>I already route most of my AIM and GTalk interaction through an IRC gateway using <a href="http:&#47;&#47;www.bitlbee.org&#47;">BitlBee<&#47;a>, and I wanted to hook HipChat up to an IRC client as well. Here's a guide on how to do this.<&#47;p></p>
<p>(Note that I was having trouble joining channels in my LimeChat last night as I was writing this up, but I might just be missing something. Try it out!)<&#47;p></p>
<p>All the uppercase items come from the <a href="https:&#47;&#47;www.hipchat.com&#47;account&#47;xmpp">XMPP Account Settings page<&#47;a> in your HipChat account.<&#47;p></p>
<ol>
<li>Add the account in Bitlbee.
<ul>
<li><code>account add jabber USERNAME@chat.hipchat.com 'PASSWORD'<&#47;code><&#47;li><br />
<&#47;ul><br />
<&#47;li></p>
<li>Before you connect, set it up so that folks' chat names will display properly.
<ul>
<li><code>account hipchat set nick_source full_name<&#47;code><&#47;li><br />
<&#47;ul><br />
<&#47;li></p>
<li>To prevent being spammed with room history every time you join or change status, set your resource to 'bot'.
<ul>
<li><code>account hipchat set resource bot<&#47;code><&#47;li><br />
<&#47;ul><br />
<&#47;li></p>
<li>Connect to the account.
<ul>
<li><code>account hipchat on<&#47;code><&#47;li><br />
<&#47;ul><br />
<&#47;li></p>
<li>At this point, you can join the &amp;hipchat channel to see all your contacts and use &#47;msg to start talking w&#47; one of them privately.
<ul>
<li><code>&#47;join &amp;hipchat<&#47;code><&#47;li><br />
<&#47;ul><br />
<&#47;li></p>
<li>Now, add a chat. Note that the information on the XMPP settings pages doesn't give you @chat.hipchat.com. You can set whatever channel name you want.
<ul>
<li><code>chat add hipchat JABBERNAME@chat.hipchat.com #CHANNELNAME<&#47;code><&#47;li><br />
<&#47;ul><br />
<&#47;li></p>
<li>Before you can join, you must set your nick to match the <strong>Room nickname<&#47;strong> setting in your XMPP settings. You must do this for every room. Use the channel name you decided on earlier for easier access.
<ul>
<li><code>channel #CHANNELNAME set nick 'ROOM NICKNAME'<&#47;code><&#47;li><br />
<&#47;ul><br />
<&#47;li></p>
<li>Now, join the channel.
<ul>
<li><code>&#47;join #CHANNELNAME<&#47;code><&#47;li><br />
<&#47;ul><br />
<&#47;li></p>
<li>Start chatting!
<ul>
<li><code>Hey everybody!<&#47;code><&#47;li>
<li><code>I'm using IRC!<&#47;code><&#47;li>
<li><code>(fry)<&#47;code><&#47;li>
<li><code>&#47;me likes IRC and HipChat<&#47;code><&#47;li><br />
<&#47;ul><br />
<&#47;li></p>
<li>When you're ready to sign out of HipChat, use <code>account off<&#47;code>.
<ul>
<li><code>account hipchat off<&#47;code><&#47;li><br />
<&#47;ul><br />
<&#47;li><br />
<&#47;ol></p>
