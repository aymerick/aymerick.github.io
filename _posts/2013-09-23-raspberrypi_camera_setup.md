---
layout: post
title: Raspberry Pi Camera setup
tags:
- raspberrypi
- piwi.local
---

Prerequisites
=============

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})

Plug the camera
===============

Plug the camera as explained here: <http://www.youtube.com/watch?v=GImeVqHQzsE>

Update system
=============

{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get upgrade
{% endhighlight %}

Enable camera
=============

{% highlight bash %}
$ sudo raspi-config
{% endhighlight %}

Enable Camera then reboot.

Test
====

{% highlight bash %}
$ mkdir /home/pi/picam
$ cd /home/pi/picam
{% endhighlight %}

Capture an image in jpeg format:

{% highlight bash %}
$ raspistill -o image.jpg
{% endhighlight %}

Capture a 10s video:

{% highlight bash %}
$ raspivid -o video.h264 -t 10000
{% endhighlight %}

Capture a 10s video in demo mode:

{% highlight bash %}
$ raspivid -o video.h264 -t 10000 -d
{% endhighlight %}

Capture a 5s video in 1280x720 with a bitrate of 8Mb:

{% highlight bash %}
$ raspivid -o video.h264 -w 1280 -h 720 -b 8000000
{% endhighlight %}

Convert h264 to mp4:

{% highlight bash %}
$ sudo apt-get install -y gpac
$ MP4Box -fps 30 -add video.h264 video.mp4
{% endhighlight %}


Stream to VLC
=============

{% highlight bash %}
$ sudo apt-get install vlc
$ raspivid -o - -t 0 -w 640 -h 360 -fps 25 | cvlc stream:///dev/stdin --sout '#standard{access=http,mux=ts,dst=:8090}' :demux=h264
{% endhighlight %}

In VLC: Open Network URL => <http://piwi.local:8090>


Disable red led
===============

{% highlight bash %}
$ sudo emacs /boot/config.txt
{% endhighlight %}

Add line:

```
disable_camera_led=1
```

Then reboot:

{% highlight bash %}
$ sudo reboot
{% endhighlight %}


External References
===================

- <http://www.raspberrypi.org/archives/3890>
- <http://www.raspberrypi-spy.co.uk/2013/05/capturing-hd-video-with-the-pi-camera-module/>
- <http://raspi.tv/2013/how-to-stream-video-from-your-raspicam-to-your-nexus-7-tablet-using-vlc>
