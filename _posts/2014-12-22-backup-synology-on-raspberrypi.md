---
layout: post
title: Backup a Synology NAS to a raspberry pi
tags:
- raspberrypi
- pickup.local
- chezak.local
---

Prerequisites
=============

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})


Setup raspberry
===============

Add a user for `synology`:

{% highlight bash %}
$ sudo useradd synology -m -G users
$ sudo passwd synology
{% endhighlight %}

Mount USB drive on `/mnt/usbdrive`:

{% highlight bash %}
$ sudo mkdir /mnt/usbdrive

$ sudo fdisk -l
$ sudo emacs /etc/fstab
{% endhighlight %}

```
/dev/sdb1       /mnt/usbdrive   ext4    defaults,user     0       0
```

{% highlight bash %}
$ sudo mount -a
{% endhighlight %}

Create bakckup directory:

{% highlight bash %}
$ sudo mkdir /mnt/usbdrive/chezak_backup
$ sudo chown synology:users /mnt/usbdrive/chezak_backup
{% endhighlight %}


Configure synology
==================

Go to `Backup and Restore` then

  - Create: Databackup task
  - Task: Pickup Full Backup

Network Backup (rsync-compatible server)

  - Server name of IP address: 192.168.0.41
  - Username: synology
  - Password: *********
  - Backup module: /mnt/usbdrive/
  - Directory: chezak_backup

Check:

  - Enable transfer encryption
  - Enable metadata backup

Select to backup:

  - Configuration
  - Shared folder (all)

Enable backup schedule

  - Day: Daily
  - Hour(s): 03
  - Minute: 30


External references
===================

- <http://www.vdsar.net/rsync-backup-synology-remote-raspberry-pi/>
- <http://www.synology.com/en-us/support/tutorials/461>
- <http://www.prodeta.nl/synology-network-backups-to-rsync-compatible-servers/>
