---
layout: post
title: minidlna on Raspberry Pi
tags:
- raspberrypi
- pipi.local
- freebox
---

Prerequisites
=============

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})
- [NFS Client on Raspberry Pi]({% post_url 2013-09-20-nfs_client_on_raspberrypi %})

Goal
====

There are some animes on my [NAS](http://www.synology.com/) for my kids. They mainly watch them on TV thanks to the [freebox](http://en.wikipedia.org/wiki/Freebox) that acts as a DLNA client talking to the [Media Server](http://www.synology.com/dsm/home_home_applications_media_server.php) intalled on the NAS. But my NAS is a but overloaded, so I decided to replace its [Media Server](http://www.synology.com/dsm/home_home_applications_media_server.php) with a light DLNA server on a raspberry pi.

Install
=======

{% highlight bash %}
$ sudo apt-get install minidlna
{% endhighlight %}

Configure
=========

{% highlight bash %}
$ sudo emacs /etc/minidlna.conf
{% endhighlight %}

Set those lines:

```
  media_dir=V,/mnt/video/dessins_animes
  friendly_name=Dessins animes
  root_container=B
  inotify=yes
```

Restart daemon
==============

{% highlight bash %}
$ sudo /etc/init.d/minidlna restart
$ sudo /etc/init.d/minidlna force-reload
{% endhighlight %}

Note that `minidlna` reindex all files at each restart, so you have to wait a bit before trying to access the videos from the freebox.


External References
===================
- <http://doc.ubuntu-fr.org/minidlna>
