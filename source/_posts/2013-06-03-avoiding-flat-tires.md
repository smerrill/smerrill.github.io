---
layout: post
status: publish
published: true
title: Avoiding Flat Tires in Your Web Application
author: Steven Merrill
author_login: smerrill
author_email: "info@phase2technology.com"
wordpress_url: "http://www.phase2technology.com/blog/avoiding-flat-tires-in-your-web-application/"
categories: 
- caching
- cdn
- scalability
comments: []
---
This Monday, the [CitiBike bike share](http://www.citibikenyc.com/?ef_id=IBFQPPF0mFIAAFdi:20130531201408:s) launched in New York City. The website was beautiful and responsive, and more than 15,000 registrations were processed through it before the launch happened.

But then, a funny thing happened. The website and the mobile apps' maps started coming up blank, and they stayed blank for more than 12 hours. What follows should not be characterized as a failing of the technical team. Launches are tough, and I don't mean to pile on what was obviously a tough situation. Instead, I would like to look at a few choices that were made and how the system might have been better architected for scalability.

<!--more-->

## Architecture Spelunking

I have no intimate knowledge of the architecture of the Citi Bike NYC application and hosting infrastructure, but we can find out a couple things very quickly.

The misbehaving path in question was `http://www.citibikenyc.com/stations/json`. With that information in hand, we can use the `dig` and `curl` commands to get a lot of information.
```
┌┤smerrill@lilliputian-resolution [May 30 18:23:35] ~
└╼ dig +short www.citibikenyc.com
70.32.89.47
70.32.83.162
```

It looks like there's two web servers, and the site is using DNS-based load balancing. Let's take a look at the (now-fixed) JSON feed with `curl -v`. Using `curl -v` instead of `curl -I` ensures that we actually send a GET request instead of a HEAD request, just to make sure that the behavior is as close to a real browser as possible.

```
┌┤smerrill@lilliputian-resolution [May 31 12:11:59] ~
└╼ curl -v www.citibikenyc.com/stations/json &gt; /dev/null
* About to connect() to www.citibikenyc.com port 80 (#0)
* Trying 70.32.83.162...
% Total % Received % Xferd Average Speed Time Time Time Current
Dload Upload Total Spent Left Speed
0 0 0 0 0 0 0 0 --:--:-- --:--:-- --:--:-- 0* connected
* Connected to www.citibikenyc.com (70.32.83.162) port 80 (#0)
&gt; GET /stations/json HTTP/1.1
&gt; User-Agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8r zlib/1.2.5
&gt; Host: www.citibikenyc.com
&gt; Accept: */*
&gt;
&lt; HTTP/1.1 200 OK
&lt; Server: nginx
&lt; Date: Fri, 31 May 2013 16:11:59 GMT
&lt; Content-Type: text/html
&lt; Transfer-Encoding: chunked
&lt; Connection: keep-alive
&lt; Vary: Accept-Encoding
&lt; Set-Cookie: ci_session=5mh4HirmjGbrsFaoFoNf5I3MOe5%2FgpGTtterFR5ATyZIoSQewcZTqEk8CmTo1Y6A%2Bv29mRsaV7wmtMDot42z3Qo5Om5MBEVIWhVCLsBGjSWWNmyFXc4UVbNpLKIUYM3
LjeP08uWGQCw642sOgQaaZURYQlUoyBx%2F6KffPECV7IPgT7Lw8G%2FUJYzMDUHXdQDzfRenxAuMZmLpt%2BBWxUEZqCs87VWaYzhQEFnXwxSAcm4VtowNMdZZHfc8Rcw%2FSWzL4z6zJZlDhzYG0Lp%2B%
2F7rBv1wwBpqsCcSS8cXBNSW0XZc7VHJ2AuB%2BOvXzoLWwKMMH%2FHd%2FTI%2B%2BIH5Ec4O8Jjup%2Bg%3D%3D; expires=Fri, 31-May-2013 16:21:54 GMT; path=/
&lt; Vary: Accept-Encoding,User-Agent
&lt; X-Apache-Server: www1.citibikenyc.com
&lt; MS-Author-Via: DAV
&lt;
{ [data not shown]
100 116k 0 116k 0 0 853k 0 --:--:-- --:--:-- --:--:-- 993k
* Connection #0 to host www.citibikenyc.com left intact
* Closing connection #0
```

Just based on that, it looks like we can deduce several salient points about the architecture:

*   There are two DNS addresses for the backend web servers, www1.citibikenyc.com and www2.citibikenyc.com.
*   The www.citibikenyc.com DNS entry contains both the www1 and www2 addresses.
*   The servers appear to be running nginx on port 80 and then connecting to a local Apache server based on the `X-Apache-Server` header.
*   The site is built using the CodeIgniter PHP framework (the main site appears to be using the Fuel CMS atop CodeIgniter.)
*   A quick trip to [arin](http://whois.arin.net/) lets us know that those two IP addresses point to machines in a MediaTemple datacenter, so no CDN is in play.
*   Each request to the `/stations/json` path returns a Set-Cookie header that sets a CodeIgniter session cookie with a 10 minute lifetime.
For comparison, this is what the response looked like during the outage. The main differences are that during the outage, the feed was still returning a 200 HTTP response code, but the Content-Length was 0, and indeed the response body was blank, like so:

```
┌┤smerrill@lilliputian-resolution [May 27 11:34:46] ~
└╼ curl -v www.citibikenyc.com/stations/json &gt; /dev/null
* About to connect() to www.citibikenyc.com port 80 (#0)
* Trying 70.32.83.162...
% Total % Received % Xferd Average Speed Time Time Time Current
Dload Upload Total Spent Left Speed
0 0 0 0 0 0 0 0 --:--:-- --:--:-- --:--:-- 0* connected
* Connected to www.citibikenyc.com (70.32.83.162) port 80 (#0)
&gt; GET /stations/json HTTP/1.1
&gt; User-Agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8r zlib/1.2.5
&gt; Host: www.citibikenyc.com
&gt; Accept: */*
&gt;
&lt; HTTP/1.1 200 OK
&lt; Server: nginx
&lt; Date: Mon, 27 May 2013 15:34:47 GMT
&lt; Content-Type: text/html
&lt; Content-Length: 0
&lt; Connection: keep-alive
&lt; Vary: Accept-Encoding
&lt; Set-Cookie: ci_session=x7ymouLWLEeY6%2Fo%2BudFoQHOixWyP3b9Ygp8Ocv94roUESql6Gwet1nuCVBcILmqt9DQzbFsSLLkXyOZ5qL%2Fl%2FD88F0Q0uXeLptE3zlGHxP0EISPGk5gW91SVscxi1klVRYv5Mt5zTO0KzB4obwc%2FY1AUFEodhplKXeaSURPXAw7roZVumXkmM1ALGbWQx5FF6LKm%2FtzudHm8NQPJYXDx3s3sUdVNWvWQpWe3iKEE5Su0TzqCKZcBxWYcssuPNVGEx8c5SpijHw6iR7sqnTMBnMdv7m4jsuj9ZweGk6JfGEp3G5%2BXAMqdsWfE%2Fa8449o92%2BlLug0NCFRdxH7ViZHXBA%3D%3D; expires=Mon, 27-May-2013 15:44:47 GMT; path=/
&lt; Vary: Accept-Encoding,User-Agent
&lt; X-Apache-Server: www1.citibikenyc.com
&lt; MS-Author-Via: DAV
&lt;
{ [data not shown]
0 0 0 0 0 0 0 0 --:--:-- --:--:-- --:--:-- 0
* Connection #0 to host www.citibikenyc.com left intact
* Closing connection #0<
```

## The Case of the Missing JSON

It seems the root of the problem was that the Apache server behind the nginx server was overloaded. It appears that at one point, the `/stations/json` endpoint was throwing errors based on this tweet:

> @[stevenmerrill](https://twitter.com/stevenmerrill) That @[citibikenyc](https://twitter.com/citibikenyc) feed has been returning Apache "too many connections" errors for a couple of hrs.Shows strong demand.
> 
> — codeline telemetry (@codelinegeekery) [May 27, 2013](https://twitter.com/codelinegeekery/status/339033281687343105)
&nbsp;

And then it was changed to this odd 200 response with no data after that. Naturally, that lead me to a simple question.

## Y u no cache?

Why not cache or pre-generate the JSON? Even with a TTL of 60 or 30 seconds, caching the rendered JSON instead of going back to a dynamic application for every visitor could help protect the backend LAMP stack from spikes in traffic.

I can only speculate, but I imagine that the design of this feed went something like the following:

"Our CitiBike stations will be pinging in every 30 seconds with new information about how many bikes and docks are available, so we have to be sure that the information we send to our mobile apps is as fresh as possible."

Keeping your customers up to date about if they'll be able to get or return a bike is certainly of paramount importance to a system like this, but that doesn't have to translate into dynamically generating JSON data on every request.

## Cache Rules Everything Around Me

My first thought upon seeing this situation was hasty tweet:

> Oddly, the misbehaving @[citibikenyc](https://twitter.com/citibikenyc) JSON feed returns a 200 and an empty body. Maybe the team should have written flat files and used a CDN.
> 
> — Steven Merrill (@stevenmerrill) [May 27, 2013](https://twitter.com/stevenmerrill/status/339031661952004096)
&nbsp;

There are several possible ways that the JSON response could be cached to lower the load on the backend web servers.

The lowest-risk option would be to have an out-of-band process like cron or Jenkins generate a static JSON file on the filesystem every 30 or 60 seconds. Then have nginx deliver that static JSON file to clients when they visit the `/stations/json` URL. This is a great option, since the load generated to service that endpoint would be both consistently timed (every 30 or 60 seconds) and predictable. For even more scalability beyond the two nginx servers, this file could then be pushed to a CDN, or the file could be served from [Fastly](http://www.fastly.com/), a Varnish-powered CDN with instant purging.

Another option that would allow for more finely-tuned updates is to use a message queueing system. With this design, running a task on cron or with Jenkins would not be needed. Instead, the bike docking stations would send messages to the queue whenever an action occurred that changed the number of bikes at the station (bike checked out or bike returned.) In a naive version of the system, each of these state-change messages would regenerate the whole JSON file. In a smarter version, the state-change messages would update a single cached JSON fragment representing the station, and the new JSON file could be generated from this cache so that database load would be lowered when the file had to be regenerated.

Both of these approaches are much preferred because the system load is controllable and is not tied to how many requests for the JSON file are coming in. A third server that didn't respond to web requests could do this processing.

Finally, since the application already has nginx at the frontend, another option is to use nginx's `proxy_cache` and `proxy_cache_use_stale` directives and cache the output of `/stations/json` for a certain TTL. This is not quite as ideal, as the potential exists to have TTL drift between the two web servers, such that someone might get data that was a few seconds stale based on which server they hit, but the business would probably agree that that is better than outright downtime.

For this to work, the Set-Cookie header must be removed. As far as I can tell, there is no personalized data being sent from that endpoint, so this session cookie does not serve a useful purpose.

## In Summary

The launch of a web application can be tough. Load patterns emerge in the real world that your load testing didn't take into account. Regardless, if you know that any part of your system may get a lot of traffic or very bursty traffic, you can engineer your application for resilience by moving data generation out of uncached web requests and into cron- or queue-backed update processes.

Phase2 recently experienced a challenge like this in helping the [Robin Hood Foundation](//www.phase2technology.com/client/robin-hood-foundation/) architect a website for the 12-12-12 Concert for Hurricane Sandy. If you're interested in learning more about that you can listen to the [panel talk we just gave at Drupalcon Portland](http://www.youtube.com/watch?v=itKY_lO7_Us&amp;feature=youtu.be).
