---
layout: post
title: The Millisecond magic of the browser
subtitle: 
categories: Tech
tags: [Hive, Bigdata]
excerpt_image: /assets/images/hiveserver.png
---

The instant you hit the https://google.com in your browser the google home page appears. But how is this magic happening and that too in fraction of seconds.

As you hit the search button the browser first needs to resolve the domain name to its ip for it to communicate. This happens through DNS resilution

The browser first checks whether it can resolve the ip address by checking its local cache. If it is available it fetches the ip address else it checks with the OS

The OS checks for the domain to ip mapping. If it isnt available to goes to the DNS recursive resolver

The DNS recursive resolver checks its local cache
