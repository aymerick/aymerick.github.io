---
layout: post
title: NFS Client on Raspberry Pi
tags:
- raspberrypi
- pipi.local
- chezak.local
- freebox
---

Prerequisites
=============

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})

Mount folders on NAS
====================

My goal is to share folders that are on my [NAS](http://www.synology.com/) which hostname is `chezak.local` and IP is `192.168.0.13`. So first:

- create a `pi` user/group on the NAS with the sames uids as on the pi: `1000/1000`
- setup NFS permissions on shared directories on the NAS via the web interface

It seems that `rpcbin` is a necessary dependency:

{% highlight bash %}
$ sudo apt-get install rpcbind
$ sudo update-rc.d rpcbind enable
$ sudo /etc/init.d/rpcbind start
{% endhighlight %}

Create folders:

{% highlight bash %}
$ sudo mkdir /mnt/incoming
$ sudo mkdir /mnt/music
$ sudo mkdir /mnt/video
{% endhighlight %}

Mount folders:

{% highlight bash %}
$ sudo mount chezak.local:/volume1/incoming /mnt/incoming
$ sudo mount chezak.local:/volume1/music /mnt/music
$ sudo mount chezak.local:/volume1/video /mnt/video
{% endhighlight %}

Test:

{% highlight bash %}
$ showmount -e chezak.local
{% endhighlight %}

Persist mounts:

{% highlight bash %}
$ sudo emacs /etc/fstab
{% endhighlight %}

Add those lines:

```
192.168.0.13:/volume1/incoming /mnt/incoming nfs rw,hard,intr 0 0
192.168.0.13:/volume1/music /mnt/music nfs ro,hard,intr 0 0
192.168.0.13:/volume1/video /mnt/video nfs ro,hard,intr 0 0
```


External References:

- <https://help.ubuntu.com/community/SettingUpNFSHowTo>
- <https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/3/html/System_Administration_Guide/s1-nfs-mount.html>


Mount folder on Freebox v6
==========================

First, enable windows share on [freebox](http://en.wikipedia.org/wiki/Freebox) with user/pass and `CHEZAK` workgroup.

Freebox's hostname is `mafreebox.freebox.fr`.

Test:

{% highlight bash %}
$ smbclient -L mafreebox.freebox.fr -N | grep "Disk" | cut -c -17
{% endhighlight %}

Create folder:

{% highlight bash %}
$ sudo mkdir /mnt/freebox
{% endhighlight %}

Setup mount:

{% highlight bash %}
$ sudo emacs /etc/fstab
{% endhighlight %}

Add that line:

```
//mafreebox.freebox.fr/Disque\040dur /mnt/freebox cifs _netdev,rw,users,iocharset=utf8,user=freebox,password=PASSWORD, uid=1000,sec=none,file_mode=0777,dir_mode=0777 0 0
```

Mount folder:

{% highlight bash %}
$ sudo mount /mnt/freebox
{% endhighlight %}


External References:

- <http://www.mumbly58.fr/monter-automatiquement-les-disques-de-la-freebox/>
- <http://doc.ubuntu-fr.org/freeboxv6>
