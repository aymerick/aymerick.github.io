---
layout: post
title: Headless Raspberry Pi setup
---

SDCard setup on Mac OS X
========================

Reference: <http://elinux.org/RPi_Easy_SD_Card_Setup#Using_command_line_tools_.282.29>

Download raspbian [here](http://www.raspberrypi.org/downloads).

{% highlight bash %}
$ df -h
{% endhighlight %}

Connect the SDCard.

{% highlight bash %}
$ df -h
{% endhighlight %}

Look for the new device that wasn't listed last time (eg: `/dev/disk2s1`).

{% highlight bash %}
$ sudo diskutil unmount /dev/disk2s1
$ sudo dd bs=1m if=~/Downloads/2013-05-25-wheezy-raspbian.img of=/dev/rdisk2
$ sudo diskutil eject /dev/rdisk2
{% endhighlight %}

Insert SDCard in the raspberry pi then power it.

Login is `pi` and password is `raspberry`.


Raspi config
============

{% highlight bash %}
$ sudo raspi-config
{% endhighlight %}

- Advanced Options > Update
- Expand Filesystem
- Change User Password
- Enable Boot to Desktop: No
- Internationalisation Options > Change Timezone
- Overclock > Medium
- Advanced Options > Hostname
- Advanced Options > Memory Split: 16

{% highlight bash %}
$ sudo apt-get install emacs23-nox
$ sudo reboot
{% endhighlight %}


Fix the 'Setting locale failed' issue during ssh
================================================

Reference: <http://stackoverflow.com/questions/2499794/how-can-i-fix-a-locale-warning-from-perl>

{% highlight bash %}
$ sudo emacs /etc/ssh/sshd_config
{% endhighlight %}

Comment that line:

```
AcceptEnv LANG LC_*
```

Test with:
{% highlight bash %}
$ perl -e exit
{% endhighlight %}


Optimizations
=============

{% highlight bash %}
$ sudo apt-get purge consolekit desktop-base* desktop-file-utils* gnome-icon-theme* gnome-themes-standard* hicolor-icon-theme* leafpad* lxde* lxde-core* midori* xserver-$ common* xserver-xorg* xserver-xorg-core* xserver-xorg-input-all* xserver-xorg-input-evdev* xserver-xorg-input-synaptics* xserver-xorg-video-fbdev*
$ sudo apt-get -y autoremove
$ sudo apt-get -y autoclean
{% endhighlight %}


Update
======

{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get upgrade
$ uname -a
$ sudo apt-get -y dist-upgrade
{% endhighlight %}


SSH setup
=========

Regenerate unique host public/private keys:
{% highlight bash %}
$ sudo rm /etc/ssh/ssh_host_*
$ sudo dpkg-reconfigure openssh-server
{% endhighlight %}

Add public key:
{% highlight bash %}
$ mkdir /home/pi/.ssh
$ emacs /home/pi/.ssh/authorized_keys
{% endhighlight %}

Add content of `id_rsa.pub`.

To permit login between pis, on mac:
{% highlight bash %}
$ scp id_rsa.pub pi@<IP>:/home/pi/.ssh/
{% endhighlight %}


avahi/netatalk setup
====================

References:

- <http://elinux.org/RPi_Advanced_Setup>
- <http://gettingstartedwithraspberrypi.tumblr.com/post/24398167109/file-sharing-with-afp-and-auto-discovery-with-bonjour>

Install avahi:
{% highlight bash %}
$ sudo apt-get install avahi-daemon
$ sudo insserv avahi-daemon
{% endhighlight %}

Setup conf file:
{% highlight bash %}
$ sudo emacs /etc/avahi/services/multiple.service
{% endhighlight %}

Insert:
{% highlight xml %}
<?xml version="1.0" standalone='no'?>
<!DOCTYPE service-group SYSTEM "avahi-service.dtd">
<service-group>
        <name replace-wildcards="yes">%h</name>
        <service>
                <type>_device-info._tcp</type>
                <port>0</port>
                <txt-record>model=RackMac</txt-record>
        </service>
        <service>
                <type>_ssh._tcp</type>
                <port>22</port>
        </service>
</service-group>
{% endhighlight %}

Restart avahi daemon:
{% highlight bash %}
$ sudo /etc/init.d/avahi-daemon restart
{% endhighlight %}

Install netatalk:
{% highlight bash %}
$ sudo apt-get install netatalk
{% endhighlight %}

Can be tested with:
{% highlight bash %}
$ sudo apt-get install avahi-utils
$ avahi-browse -arp
{% endhighlight %}
