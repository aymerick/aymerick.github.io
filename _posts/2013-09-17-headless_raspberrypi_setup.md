---
layout: post
kind: post
title: Headless Raspberry Pi setup
tags:
- raspberrypi
---

SDCard setup on Mac OS X
========================

Download raspbian [here](http://www.raspberrypi.org/downloads).

Connect the SDCard.

{% highlight bash %}
$ df -h
$ diskutil list
{% endhighlight %}

Look for the new device that wasn't listed last time (eg: `/dev/disk2s1`).

{% highlight bash %}
$ sudo diskutil unmount /dev/disk2s1
$ sudo dd bs=1m if=~/Downloads/2015-09-24-raspbian-jessie.img of=/dev/rdisk2
$ sudo diskutil eject /dev/rdisk2
{% endhighlight %}

Insert SDCard in the raspberry pi then power it.

Login is `pi` and password is `raspberry`.


External References:

- <http://elinux.org/RPi_Easy_SD_Card_Setup#Using_command_line_tools_.282.29>

Optional: external HDD setup
============================

This is an optional step if you want to run the system on an external hard drive (the SD card will only be used to boot).

Find your drive with:

{% highlight bash %}
$ lsusb
$ dmesg
{% endhighlight %}

Execute that script to setup everything:

{% highlight bash %}
$ git clone https://github.com/adafruit/Adafruit-Pi-ExternalRoot-Helper.git
$ cd Adafruit-Pi-ExternalRoot-Helper
$ sudo ./adafruit-pi-externalroot-helper -d /dev/sda
{% endhighlight %}

If you see errors like:

    Error: Partition(s) on /dev/sda are being used.

Then unmount partitions:

{% highlight bash %}
$ mount -l
$ sudo umount /media/pi/rootfs
$ sudo ./adafruit-pi-externalroot-helper -d /dev/sda
{% endhighlight %}

Finally, you can reboot:

{% highlight bash %}
$ sudo reboot
{% endhighlight %}

Check setup with:

{% highlight bash %}
$ df -h
$ readlink /dev/root
$ ls -l /dev/disk/by-uuid
$ ls -l /dev/disk/by-label
$ mount | grep root
{% endhighlight %}

External References:

- <https://learn.adafruit.com/external-drive-as-raspberry-pi-root/hooking-up-the-drive-and-copying-slash>


Optional: USB Stick setup [DEPRECATED]
=========================

**THIS IS DEPRECATED AND SHOULD BE REPLACED BY THE SAME METHOD AS FOR HDD SETUP**

So let's install raspbian on the USB stick.

{% highlight bash %}
$ diskutil list
{% endhighlight %}

Connect the USB stick.

{% highlight bash %}
$ diskutil list
{% endhighlight %}

Look for the new device that wasn't listed last time (eg: `/dev/disk1`).

{% highlight bash %}
$ diskutil unmountDisk /dev/disk1
$ sudo dd bs=1m if=~/Downloads/2014-09-09-wheezy-raspbian.img of=/dev/disk1
{% endhighlight %}

Eject the USB stick.

Insert the USB stick in the raspberry pi.

{% highlight bash %}
$ sudo fdisk -l
{% endhighlight %}

```
  Disk /dev/sda: 32.0 GB, 32015679488 bytes
```

Expand partition on USB Stick:

{% highlight bash %}
$ sudo fdisk /dev/sda
{% endhighlight %}

Press `p`:

```
  Device Boot      Start         End      Blocks   Id  System
  /dev/sda1         8192      122879       57344    c  W95 FAT32 (LBA)
  /dev/sda2       122880     6399999     3138560   83  Linux
```

Note the start of `/dev/sda2`: `122880`.

Press `d`, type `2` and then hit `return` to delete the `sda2` partition.

Then create a new partition (but leave 10Mb free):

```
  Command (m for help): n
  Partition type:
     p   primary (1 primary, 0 extended, 3 free)
     e   extended
  Select (default p): p
  Partition number (1-4, default 2): 2
  First sector (2048-62530623, default 2048): 122880
  Last sector, +sectors or +size{K,M,G} (122880-62530623, default 62530623): 62528623
```

The math:

```
  10240000 / 512 = 2000 sectors
  62530623 - 2000 = 62528623
```

Press `w` to commit changes and exit fdisk.

Reboot the raspberry:

{% highlight bash %}
$ sudo reboot
{% endhighlight %}

Resize the FS:

{% highlight bash %}
$ e2fsck -f /dev/sda2
$ sudo resize2fs /dev/sda2
{% endhighlight %}

Convert the partition table from DOS to GPT:

{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get install gdisk
$ sudo gdisk /dev/sda
{% endhighlight %}

```
  GPT fdisk (gdisk) version 0.8.5

  Partition table scan:
    MBR: MBR only
    BSD: not present
    APM: not present
    GPT: not present


  ***************************************************************
  Found invalid GPT and valid MBR; converting MBR to GPT format.
  THIS OPERATION IS POTENTIALLY DESTRUCTIVE! Exit by typing 'q' if
  you don't want to convert your MBR partitions to GPT format!
  ***************************************************************

  Command (? for help): r

  Recovery/transformation command (? for help): f
  Warning! This will destroy the currently defined partitions! Proceed? (Y/N): Y

  Recovery/transformation command (? for help): w

  Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
  PARTITIONS!!

  Do you want to proceed? (Y/N): Y
  OK; writing new GUID partition table (GPT) to /dev/sda.
  The operation has completed successfully.
```

Now remove USB stick, then re-plug it.

{% highlight bash %}
$ sudo gdisk /dev/sda
{% endhighlight %}

```
  GPT fdisk (gdisk) version 0.8.5

  Partition table scan:
    MBR: protective
    BSD: not present
    APM: not present
    GPT: present

  Found valid GPT with protective MBR; using GPT.

  Command (? for help): i
  Partition number (1-2): 2
  Partition GUID code: 0FC63DAF-8483-4772-8E79-3D69D8477DE4 (Linux filesystem)
  Partition unique GUID: D7BBB26D-DD33-4333-8CF3-0AA7F3517B48
  First sector: 122880 (at 60.0 MiB)
  Last sector: 62528623 (at 29.8 GiB)
  Partition size: 62405744 sectors (29.8 GiB)
  Attribute flags: 0000000000000000
  Partition name: 'Linux filesystem'
```

Note the `Partition unique GUID`: D7BBB26D-DD33-4333-8CF3-0AA7F3517B48

{% highlight bash %}
$ sudo emacs /boot/cmdline.txt
{% endhighlight %}

```
  dwc_otg.lpm_enable=0 console=ttyAMA0,115200 console=tty1 root=PARTUUID=D7BBB26D-DD33-4333-8CF3-0AA7F3517B48 rootfstype=ext4 elevator=deadline rootwait
```

{% highlight bash %}
$ sudo reboot
{% endhighlight %}

External References:

- <http://www.raspipress.com/2013/05/install-and-run-raspbian-from-a-usb-flash-drive/>
- <https://samhobbs.co.uk/2013/10/speed-up-your-pi-by-booting-to-a-usb-flash-drive>
- <http://blog.krastanov.org/2014/01/30/booting-pi-reliably-from-usb/>


Raspi config
============

{% highlight bash %}
$ sudo raspi-config
{% endhighlight %}

- Advanced Options > Update
- Expand Filesystem
- Change User Password
- Boot Options > Console
- Internationalisation Options > Change Timezone
- Overclock > Medium (or: Pi2)
- Advanced Options > Hostname
- Advanced Options > Memory Split: 16

{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get install emacs23-nox
$ sudo reboot
{% endhighlight %}


Fix the 'Setting locale failed' issue during ssh
================================================

{% highlight bash %}
$ sudo emacs /etc/ssh/sshd_config
{% endhighlight %}

Comment that line:

```
AcceptEnv LANG LC_*
```

To test:
{% highlight bash %}
$ sudo /etc/init.d/ssh restart
$ exit
$ ssh pi@<PI IP>
$ perl -e exit
{% endhighlight %}


External References:

- <http://stackoverflow.com/questions/2499794/how-can-i-fix-a-locale-warning-from-perl>


Optimizations
=============

{% highlight bash %}
$ sudo apt-get purge consolekit desktop-base* desktop-file-utils* gnome-icon-theme* gnome-themes-standard* hicolor-icon-theme* leafpad* lxde* lxde-core* midori* xserver-common* xserver-xorg* xserver-xorg-core* xserver-xorg-input-all* xserver-xorg-input-evdev* xserver-xorg-input-synaptics* xserver-xorg-video-fbdev* gconf gconf2 gconf2-common gnome-accessibility-themes libmenu-cache1 lxappearance lxinput lxmenu-data lxpanel lxpolkit lxrandr lxsession lxsession-edit lxshortcut lxtask lxterminal scratch
$ sudo apt-get -y remove cups* gnome* x11-common*
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


avahi/netatalk setup
====================

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


External References:

- <http://elinux.org/RPi_Advanced_Setup>
- <http://gettingstartedwithraspberrypi.tumblr.com/post/24398167109/file-sharing-with-afp-and-auto-discovery-with-bonjour>
