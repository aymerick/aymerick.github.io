---
layout: post
title: BerryCam on Raspberry Pi
tags:
- raspberrypi
- piwi.local
---

No video, only photo :/

Prerequisites
=============

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})
- [Raspberry Pi Camera setup]({% post_url 2013-09-23-raspberrypi_camera_setup %})

Download iOs app
================

=> https://itunes.apple.com/gb/app/berrycam/id687071023?mt=8

Launch daemon on Raspberry Pi
=============================

{% highlight bash %}
$ wget https://bitbucket.org/fotosyn/fotosynlabs/raw/9819edca892700e459b828517bba82b0984c82e4/BerryCam/berryCam.py
$ sudo python berryCam.py
{% endhighlight %}

External References
===================

- <http://www.fotosyn.com/berrycam-support/>
