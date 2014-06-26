---
layout: post
title: Domoticz
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
$ sudo apt-get install git-core subversion screen
$ sudo apt-get install cmake make gcc g++ libboost-dev libboost-thread-dev libboost-system-dev libsqlite3-dev curl libcurl4-openssl-dev libusb-dev

$ mkdir domoticz
$ cd domoticz
$ wget http://domoticz.sourceforge.net/domoticz_linux_armv6l.tgz
$ tar xvfz domoticz_linux_armv6l.tgz
$ rm domoticz_linux_armv6l.tgz

$ sudo cp domoticz.sh /etc/init.d
$ sudo chmod +x /etc/init.d/domoticz.sh
$ sudo update-rc.d domoticz.sh defaults

$ sudo service domoticz.sh start
{% endhighlight %}

Browse: http://pikan.local:8080


Remove 'zero temperature' graph points
======================================

{% highlight bash %}
$ sudo apt-get install sqlite3

$ sudo service domoticz.sh stop
$ sudo sqlite3 domoticz/domoticz.db
{% endhighlight %}

```
sqlite> .headers ON
sqlite> .mode tabs
sqlite> SELECT * FROM Temperature_Calendar WHERE DeviceRowID = 10;

sqlite> DELETE FROM Temperature_Calendar WHERE DeviceRowID = 10 AND Temp_Min=0;
sqlite> DELETE FROM Temperature WHERE DeviceRowID = 10 AND Temperature = 0;

sqlite> .quit
```

{% highlight bash %}
$ sudo service domoticz.sh start
{% endhighlight %}


JSON API
========

$ curl -s -i -H "Accept: application/json" "http://pikan.local:8080/json.htm?type=command&param=udevice&idx=2&nvalue=0&svalue=12.3;45;0"

References:
- http://www.domoticz.com/wiki/Preparing_RaspberryPI_(Debian-Wheezy)_for_Domoticz
- http://www.domoticz.com/wiki/Installing_and_running_Domoticz_on_a_Raspberry_PI
- http://www.domoticz.com/wiki/Domoticz_API/JSON_URL%27s
