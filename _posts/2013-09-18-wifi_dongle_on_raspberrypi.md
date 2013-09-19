---
layout: post
title: TL-WN725N wifi dongle on Raspberry Pi
tags:
- raspberrypi
- piwi
---

References:

- <http://www.raspberrypi.org/phpBB3/viewtopic.php?f=65&t=43398>
- <http://burogu.makotoworkshop.org/index.php?post/2013/04/27/TL-WN725N-v2>
- <http://blog.pi3g.com/2013/05/tp-link-tl-wn725n-nano-wifi-adapter-v2-0-raspberry-pi-driver/>

Network setup
=============

{% highlight bash %}
$ sudo nano /etc/network/interfaces
{% endhighlight %}

Set:

```
auto lo

iface lo inet loopback
iface eth0 inet dhcp

allow-hotplug wlan0
iface wlan0 inet dhcp
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
iface default inet dhcp
```

WPA setup
=========

{% highlight bash %}
$ sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
{% endhighlight %}

Set:

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
 ssid="chezak_ng"
 proto=RSN
 key_mgmt=WPA-PSK
 pairwise=CCMP TKIP
 group=CCMP TKIP
 psk="<PASSWORD>"
}
```

Fix TL-WN725N v2.0 issue
========================

{% highlight bash %}
$ wget https://dl.dropboxusercontent.com/u/80256631/8188eu-20130209.tar.gz
$ tar -zxvf 8188eu-20130209.tar.gz
$ sudo install -p -m 644 8188eu.ko /lib/modules/3.6.11+/kernel/drivers/net/wireless
$ sudo depmod -a
$ sudo modprobe 8188eu
$ ifconfig -a
$ sudo reboot
{% endhighlight %}

If wifi becomes unavailable after an update, run:
{% highlight bash %}
$ sudo depmod -a
$ sudo modprobe 8188eu
{% endhighlight %}
