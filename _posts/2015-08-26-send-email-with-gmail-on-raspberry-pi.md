---
layout: post
kind: post
title: Send email with gmail on raspberry pi
tags:
- raspberrypi
- pickup.local
---

# Prerequisites

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})


# Setup

Install [ssmtp](https://wiki.debian.org/sSMTP):

{% highlight bash %}
$ sudo apt-get install ssmtp
$ sudo emacs /etc/ssmtp/ssmtp.conf
{% endhighlight %}

```
root=YOUR_EMAIL
mailhub=smtp.gmail.com:587
hostname=PI_HOSTNAME
AuthUser=YOUR_EMAIL
AuthPass=YOUR_PASSWORD
FromLineOverride=YES
UseSTARTTLS=YES
```

Install [heirloom-mailx](http://heirloom.sourceforge.net/mailx.html):

{% highlight bash %}
$ sudo apt-get install heirloom-mailx
{% endhighlight %}


# Test

Send a mail to `DESTINATION_EMAIL`:

{% highlight bash %}
$ echo This is a test | mail -v -s "A mail from your pi" DESTINATION_EMAIL
{% endhighlight %}


# External references

- <http://doc.ubuntu-fr.org/ssmtp>
- <http://blog.hotfirenet.com/envoyer-mail-depuis-le-raspberry-pi/>
