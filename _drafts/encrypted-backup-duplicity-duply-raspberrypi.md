---
layout: post
title: Encrypted backup with Duplicity and Duply on raspberrypi
tags:
- raspberrypi
- pickup.local
- pipote.local
---

Prerequisites
=============

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})

Note that this is a backup of another Synology backup (see ({% post_url 2013-09-17-headless_raspberrypi_setup %})):

```
Synology NAS => Rsync backup on first Raspberry => Duplicity backup on second Raspberry
```

So this is a very specific conf.


Setup target raspberry
======================

Add a user for `synology`:

{% highlight bash %}
$ sudo useradd synology -m -G users
$ sudo passwd synology
{% endhighlight %}

Format hard drive if necessary:

{% highlight bash %}
$ sudo mkfs.ext4 /dev/sda1 -L pipote
{% endhighlight %}

Mount USB drive on `/mnt/usbdrive`:

{% highlight bash %}
$ sudo mkdir /mnt/usbdrive

$ sudo fdisk -l
$ sudo emacs /etc/fstab
{% endhighlight %}

```
/dev/sda1       /mnt/usbdrive   ext4    defaults,user     0       0
```

{% highlight bash %}
$ sudo mount -a
{% endhighlight %}

Create backup directory:

{% highlight bash %}
$ sudo mkdir -p /mnt/usbdrive/backup/encrypted
$ sudo chown synology:synology /mnt/usbdrive/backup
{% endhighlight %}


Setup source raspberry
======================

Generate SSH keys:

{% highlight bash %}
$ ssh-keygen -t rsa -N ''
{% endhighlight %}

On target, adds content of source's `/home/pi/.ssh/id_rsa.pub` file to `/home/synology/.ssh/authorized_keys` file.

Install duplicity and duply:

{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get install duplicity duply python-gobject-2
{% endhighlight %}

Create a profile named `pipote`:

{% highlight bash %}
$ duply pipote create
{% endhighlight %}

Generate a GPG key:

{% highlight bash %}
$ gpg --gen-key
{% endhighlight %}

Generation will be blocked on message:

```
Not enough random bytes available.  Please do some other work to give
the OS a chance to collect more entropy! (Need 284 more bytes)
```

Open a new terminal and install a package to generate entropy. For example:

{% highlight bash %}
$ sudo apt-get install haveged
{% endhighlight %}

{% highlight bash %}
View geneated key:
{% endhighlight %}

$ gpg --list-keys

Setup duply conf:

{% highlight bash %}
$ emacs /home/pi/.duply/pipote/conf
{% endhighlight %}

```
GPG_KEY='<GPG KEY>'
GPG_PW='<GPG KEY PASSPHRASE>'
TARGET='rsync://synology@<TARGET IP>//mnt/usbdrive/backup/encrypted'
SOURCE='/mnt/usbdrive/chezak_backup'
```

Choose files to backup:

{% highlight bash %}
$ emacs /home/pi/.duply/pipote/exclude
{% endhighlight %}

```
- **/@app
- **/@eaDir
+ /mnt/usbdrive/chezak_backup/homes
+ /mnt/usbdrive/chezak_backup/time_machine
- **
```

Start first backup within `screen`:

{% highlight bash %}
$ sudo apt-get install screen
$ screen

$ duply pipote backup
{% endhighlight %}

When first backup is over, copy the profile folder `/home/pi/.duply/pipote` to a safe place.

@todo Create a CRON job to backup every week


External References
===================

- <http://duplicity.nongnu.org>
- <http://duply.net>
- <http://aguslr.github.io/blog/2012/04/18/backups-with-duply/>
- <http://blog.rom1v.com/2013/08/duplicity-des-backups-incrementaux-chiffres>
