---
layout: post
kind: post
title: Sickbeard on Raspberry Pi
tags:
- raspberrypi
- freebox
---

Prerequisites
=============

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})
- [NFS Client on Raspberry Pi]({% post_url 2013-09-20-nfs_client_on_raspberrypi %})

Install
=======

{% highlight bash %}
$ sudo apt-get install python-cheetah
$ cd
$ git clone git://github.com/midgetspy/Sick-Beard.git
$ cd Sick-Beard
$ python SickBeard.py
{% endhighlight %}

Configure
=========

Browse to: <http://IP:8081>

Config > General
----------------

  - Uncheck `Launch Browser`

Config > Search Settings
------------------------

  - Uncheck `Search NZBs`
  - Check `Search Torrents`
  - Torrent Black Hole: `/mnt/freebox/Téléchargements/A Télécharger`

Config > Search Provides
------------------------

  - Check EZRSS

Config > Post Processing
------------------------

  - TV download dir: `/mnt/freebox/Téléchargements`
  - Uncheck `Keep Original Files`
  - Check `Move Associated Files`
  - Uncheck `Rename episodes`
  - Check `Scan and Process`

Setup daemon
============

{% highlight bash %}
$ sudo emacs /etc/init.d/sickbeard
{% endhighlight %}

Set content:

{% highlight bash %}
#! /bin/sh

# Author: daemox
# Basis: Parts of the script based on and inspired by work from
#tret (sabnzbd.org), beckstown (xbmc.org),
#and midgetspy (sickbeard.com).
# Fixes: Alek (ainer.org), James (ainer.org), Tophicles (ainer.org),
#croontje (sickbeard.com)
# Contact: http://www.ainer.org
# Version: 3.1

### BEGIN INIT INFO
# Provides:          sickbeard
# Required-Start:    $local_fs $network $remote_fs
# Required-Stop:     $local_fs $network $remote_fs
# Should-Start:      $NetworkManager
# Should-Stop:       $NetworkManager
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: starts and stops sick beard
# Description:       Sick Beard is an Usenet PVR. For more information see:
#http://www.sickbeard.com
### END INIT INFO

#Required -- Must Be Changed!
USER="pi" #Set Linux Mint, Ubuntu, or Debian user name here.

#Required -- Defaults Provided (only change if you know you need to).
HOST="127.0.0.1" #Set Sick Beard address here.
PORT="8081" #Set Sick Beard port here.

#Optional -- Unneeded unless you have added a user name and password to Sick Beard.
SBUSR="" #Set Sick Beard user name (if you use one) here.
SBPWD="" #Set Sick Beard password (if you use one) here.

#Script -- No changes needed below.
case "$1" in
start)
#Start Sick Beard and send all messages to /dev/null.
cd /home/$USER/Sick-Beard
echo "Starting Sick Beard"
sudo -u $USER -EH nohup python /home/$USER/Sick-Beard/SickBeard.py -q > /dev/null 2>&1 &
;;
stop)
#Shutdown Sick Beard and delete the index.html files that wget generates.
echo "Stopping Sick Beard"
wget -q --user=$SBUSR --password=$SBPWD "http://$HOST:$PORT/home/shutdown/" --delete-after
sleep 6s
;;
*)
echo "Usage: $0 {start|stop}"
exit 1
esac

exit 0
{% endhighlight %}

Start daemon
============

{% highlight bash %}
$ sudo chmod a+x /etc/init.d/sickbeard
$ sudo update-rc.d sickbeard defaults
$ sudo /etc/init.d/sickbeard start
{% endhighlight %}


External References
===================

- <http://www.howtogeek.com/146410/how-to-automate-your-always-on-raspberry-pi-download-box/all/>
- <http://www.ainer.org/sick-beard-install-setup-configuration-guide-for-ubuntu-linux-mint>
