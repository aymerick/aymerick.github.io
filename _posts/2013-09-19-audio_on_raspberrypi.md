---
layout: post
title: Audio on Raspberry Pi
tags:
- raspberrypi
- piwi.local
---

Sound tests
===========

List sound devices:

{% highlight bash %}
$ aplay -L
{% endhighlight %}

Play a sound on default RPI output:

{% highlight bash %}
$ aplay /usr/share/scratch/Media/Sounds/Vocals/Singer2.wav
{% endhighlight %}


Install a DAC
=============

I bought [this USB DAC](http://www.ebay.fr/itm/New-PCM2704-USB-DAC-USB-to-S-PDIF-Sound-Card-Decoder-Board-with-Aluminum-Housing-/160945210698) but it is cheaper [without the enclosure](http://www.ebay.fr/itm/New-PCM2704-USB-DAC-USB-to-S-PDIF-Sound-Card-Decoder-Board-3-5mm-Analog-Output-/160945208556).

Play on USB DAC:

{% highlight bash %}
$ aplay -D default:CARD=DAC /usr/share/scratch/Media/Sounds/Vocals/Singer2.wav
{% endhighlight %}

Set USB DAC as default output:

{% highlight bash %}
$ sudo emacs /etc/modprobe.d/alsa-base.conf
{% endhighlight %}

Comment that line:

```
options snd-usb-audio index=-2
```

Add that line:

```
options snd-usb-audio nrpacks=1
```

Restart daemon:

{% highlight bash %}
$ sudo /etc/init.d/alsasound restart
{% endhighlight %}


Install squeezelite
===================

I already have a [Logitec Media Server](http://en.wikipedia.org/wiki/Logitech_Media_Server) installed on my [NAS](http://www.synology.com/) for my [Squeezebox Radio](http://en.wikipedia.org/wiki/Squeezebox_(network_music_player). So let's install [SqueezeLite](https://code.google.com/p/squeezelite/) client on the raspberry pi.

Reference: <http://www.raspberrypi.org/phpBB3/viewtopic.php?f=38&t=23108>

Install dependencies:

{% highlight bash %}
$ sudo apt-get install libflac-dev
$ sudo apt-get install libfaad2
$ sudo apt-get install libmad0
{% endhighlight %}

Install squeezelite:

{% highlight bash %}
$ mkdir squeezelite
$ cd squeezelite
$ wget http://squeezelite.googlecode.com/files/squeezelite-armv6hf
$ sudo mv squeezelite-armv6hf /usr/bin
$ chmod u+x /usr/bin/squeezelite-armv6hf
{% endhighlight %}

Test with:

{% highlight bash %}
$ squeezelite-armv6hf -l
$ squeezelite-armv6hf -o default:CARD=ALSA -a 200
$ squeezelite-armv6hf -n "Piwi" -a 200
{% endhighlight %}

Install daemon:

{% highlight bash %}
$ sudo wget http://www.gerrelt.nl/RaspberryPi/squeezelitehf.sh
$ sudo mv squeezelitehf.sh /etc/init.d/squeezelite
$ sudo chmod a+x /etc/init.d/squeezelite
{% endhighlight %}

Fix daemon script:

{% highlight bash %}
$ sudo emacs /etc/init.d/squeezelite
{% endhighlight %}

Remove 'SLMAC' stuff and set:

```
SLOPTIONS="-a 200 -n Piwi"
```

Start daemon:

{% highlight bash %}
$ sudo update-rc.d squeezelite defaults
$ sudo service squeezelite start
{% endhighlight %}


Install AirPlay
===============

Now I want to stream the music stored on my [NAS](http://www.synology.com/) to the raspberry pi through [Audio Station](http://www.synology.com/dsm/home_home_applications_audio_station.php), controlling it with [DS Audio](http://www.synology.com/dsm/home_mobile_support_ds_audio.php).

Thanks to [ShairPort](https://github.com/abrasive/shairport) that's easy.

References:

- <http://pihomeserver.wordpress.com/2012/12/05/raspberry-pi-home-server-etape-6-installer-un-serveur-airplay-avec-shairport/>
- <http://blog.nicolargo.com/2013/03/utiliser-votre-raspberry-pi-comme-borne-airplay.html>

Install dependencies:

{% highlight bash %}
$ sudo apt-get install git libao-dev libssl-dev libcrypt-openssl-rsa-perl libio-socket-inet6-perl libwww-perl avahi-utils
$ sudo cpan install Net::SDP
{% endhighlight %}

Install ShairPort:

{% highlight bash %}
$ git clone https://github.com/albertz/shairport.git shairport
$ cd shairport
$ make
$ sudo make install
{% endhighlight %}

Test:

{% highlight bash %}
$ shairport.pl -a ShairPort
{% endhighlight %}

Install daemon:

{% highlight bash %}
$ sudo cp ./shairport.init.sample /etc/init.d/shairport
$ sudo chmod a+x /etc/init.d/shairport
{% endhighlight %}

Fix daemon:

{% highlight bash %}
$ emacs /etc/init.d/shairport
{% endhighlight %}

Change name: `NAME=ShairPort`

Start daemon:

{% highlight bash %}
$ sudo update-rc.d shairport defaults
$ sudo service shairport start
{% endhighlight %}
