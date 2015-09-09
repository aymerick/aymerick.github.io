---
layout: post
title: rsnapshot on Raspberry Pi
tags:
- raspberrypi
- pickup.local
---

Let's backup the synology backup !


# Prerequisites

- [Backup a Synology NAS to a raspberry pi]({% post_url 2014-12-22-backup-synology-on-raspberrypi %})


# Setup new raspberry

First:

    - [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})
    - [Send email with gmail on raspberry pi](% post_url 2015-08-26-send-email-with-gmail-on-raspberry-pi %)

Then add a user for `synology`:

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

Create backup directory:

{% highlight bash %}
$ sudo mkdir /mnt/usbdrive/backup
$ sudo chmod 700 /mnt/usbdrive/backup
{% endhighlight %}


# Setup rsnapshot

Install:

{% highlight bash %}
$ sudo apt-get install rsnapshot
{% endhighlight %}

Configure:

{% highlight bash %}
$ sudo cp /etc/rsnapshot.conf /etc/rsnapshot.conf.BAK
$ sudo emacs /etc/rsnapshot.conf
{% endhighlight %}

```
snapshot_root	/mnt/usbdrive/backup/
no_create_root	1

cmd_ssh	/usr/bin/ssh
cmd_du	/usr/bin/du
cmd_rsnapshot_diff	/usr/bin/rsnapshot-diff

#retain	hourly	6
#retain	daily	7
retain	weekly	4
retain	monthly	2

verbose	3

logfile /var/log/rsnapshot.log

ssh_args	-i /root/.ssh/rsnapshot_dsa

rsync_long_args --delete --numeric-ids --delete-excluded --stats

exclude /@app
exclude /incoming
exclude /installs
exclude /video
exclude @eaDir

#backup /home/	localhost/
#backup /etc/	localhost/
#backup /usr/local/	localhost/

backup  synology@pickup.local:/mnt/usbdrive/chezak_backup/	chezak/
#backup synology@<REMOTE_SERVER>:/mnt/usbdrive/chezak_backup/ chezak/
```

Meaning:

	- keep the 4 last "weekly" backups
	- keep the 2 last "monthly" backups


# Setup reporting

{% highlight bash %}
$ sudo cp /usr/share/doc/rsnapshot/examples/utils/rsnapreport.pl.gz /usr/local/bin
$ sudo gunzip /usr/local/bin/rsnapreport.pl.gz
$ sudo chmod a+x /usr/local/bin/rsnapreport.pl
{% endhighlight %}


# Setup CRON jobs

{% highlight bash %}
$ sudo emacs /etc/cron.d/rsnapshot
{% endhighlight %}

```
# 0 */4         * * *           root    /usr/bin/rsnapshot hourly
# 30 3          * * *           root    /usr/bin/rsnapshot daily
30 1    * * 1           root    /usr/bin/rsnapshot weekly 2>&1 | /usr/local/bin/rsnapreport.pl | mail -s "[pipote] rsnapshot weekly" MY@EMAIL
35 3    1 * *           root    /usr/bin/rsnapshot monthly 2>&1 | /usr/local/bin/rsnapreport.pl | mail -s "[pipote] rsnapshot monthly" MY@EMAIL
```

Meaning:

	- launch "weekly" backup every monday at 1:30 AM
	- launch "monthly" backup every first day of the month at 3:35


# Test config

{% highlight bash %}
$ sudo rsnapshot configtest
{% endhighlight %}


# Setup ssh keys

{% highlight bash %}
$ sudo ssh-keygen -N "" -f /root/.ssh/rsnapshot_dsa
$ sudo ssh-copy-id -i /root/.ssh/rsnapshot_dsa.pub synology@pickup.local
{% endhighlight %}

Test with:
{% highlight bash %}
$ sudo ssh synology@pickup.local -i /root/.ssh/rsnapshot_dsa
{% endhighlight %}


# Manually sync

First, to speed up first sync, plug the hard drive on `pickup.local` then:

{% highlight bash %}
$ sudo mkdir /mnt/usbdrive2
$ sudo fdisk -l
$ sudo emacs /etc/fstab
{% endhighlight %}

```
/dev/sdc1       /mnt/usbdrive2   ext4    defaults,user     0       0
```

{% highlight bash %}
$ sudo mount -a
{% endhighlight %}

Launch sync:

{% highlight bash %}
$ sudo mkdir -m 0755 -p /mnt/usbdrive2/backup/weekly.0/

$ screen
$ sudo /usr/bin/rsync -av --delete --numeric-ids --delete-excluded --no-relative /mnt/usbdrive/chezak_backup/ /mnt/usbdrive2/backup/weekly.0/chezak/
{% endhighlight %}

Then, when sync is finished, remove `usbdrive2` mount point from `/etc/fstab` file.

{% highlight bash %}
$ sudo umount /mnt/usbdrive2
{% endhighlight %}

Plug back the hard drive on new raspberry.


# Trigger first backup

{% highlight bash %}
$ sudo rsnapshot -t weekly
{% endhighlight %}

Then trigger rsnapshot backup:

{% highlight bash %}
$ sudo apt-get install screen
$ screen
$ sudo rsnapshot -v weekly
{% endhighlight %}

Check disk usage:

{% highlight bash %}
$ sudo rsnapshot du
{% endhighlight %}
