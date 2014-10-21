---
layout: post
title: Logitec Media Server on a raspberry pi
tags:
- raspberrypi
- pikan.local
---

Prerequisites
=============

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})


Install
=======

{% highlight bash %}
$ sudo apt-get install libjpeg8 libpng12-0 libgif4 libexif12 libswscale2 libavcodec53
$ wget http://downloads.slimdevices.com/LogitechMediaServer_v7.7.3/logitechmediaserver_7.7.3_all.deb
$ sudo dpkg -i logitechmediaserver_7.7.3_all.deb
{% endhighlight %}


Fix install
===========

{% highlight bash %}
$ sudo service logitechmediaserver stop

$ wget http://allthingspi.webspace.virginmedia.com/files/lms-rpi-raspbian.tar.gz
$ tar -zxvf lms-rpi-raspbian.tar.gz
$ sudo patch /usr/share/perl5/Slim/bootstrap.pm lms-rpi-bootstrap.patch

$ sudo mv arm-linux-gnueabihf-thread-multi-64int /usr/share/squeezeboxserver/CPAN/arch/5.14/
$ sudo mv libmediascan.so.0.0.0 libfaad.so.2.0.0 /usr/local/lib
$ sudo mv /usr/share/squeezeboxserver/Bin/arm-linux/faad /usr/share/squeezeboxserver/Bin/arm-linux/faad.old
$ sudo mv faad /usr/share/squeezeboxserver/Bin/arm-linux
$ sudo ln -s /usr/local/lib/libmediascan.so.0.0.0 /usr/local/lib/libmediascan.so
$ sudo ln -s /usr/local/lib/libmediascan.so.0.0.0 /usr/local/lib/libmediascan.so.0
$ sudo ln -s /usr/local/lib/libfaad.so.2.0.0 /usr/local/lib/libfaad.so
$ sudo ln -s /usr/local/lib/libfaad.so.2.0.0 /usr/local/lib/libfaad.so.2
$ sudo ldconfig

$ sudo chown -R squeezeboxserver:nogroup /usr/share/squeezeboxserver/

$ sudo service logitechmediaserver start
{% endhighlight %}

Browse to: <http://pikan.local:9000>


External references
===================

- <http://allthingspi.webspace.virginmedia.com/lms.php>
- <http://wiki.slimdevices.com/index.php/Logitech_Media_Server>
