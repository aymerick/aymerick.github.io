---
layout: post
kind: post
title: Munin Server on a Raspberry Pi
tags:
- raspberrypi
---

Prerequisites
=============

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})
- [Nginx on Raspberry Pi]({% post_url 2013-12-23-nginx_on_raspberrypi %})


Install
=======

{% highlight bash %}
$ sudo apt-get install munin munin-node munin-plugins-extra
{% endhighlight %}

Setup Server
============

{% highlight bash %}
$ sudo emacs /etc/munin/munin.conf
{% endhighlight %}

```
[pikan.local]
    address 127.0.0.1
    use_node_name yes
```

{% highlight bash %}
$ sudo emacs /etc/nginx/sites-available/pikan
{% endhighlight %}

{% highlight nginx %}
server {
    listen 80;
    root /var/www;
    index index.html index.htm;

    location /stats {
        stub_status on;
        access_log off;
        allow 127.0.0.1;
        allow 192.168.0.0/24;
        deny all;
    }

    location /munin {
        alias /var/cache/munin/www;
        index index.html;
    }
}
{% endhighlight %}

{% highlight bash %}
$ sudo service nginx reload
{% endhighlight %}


Test
====

Browse: <http://pikan.local/munin>

RDD data is stored in: /var/lib/munin


Setup Node
==========

Install pi sensors:

{% highlight bash %}
$ cd /usr/share/munin/plugins
$ sudo wget -O pisense_ https://raw.github.com/perception101/pisense/master/pisense_
$ sudo chmod a+x pisense_
$ sudo ln -s /usr/share/munin/plugins/pisense_ /etc/munin/plugins/pisense_temp
$ sudo ln -s /usr/share/munin/plugins/pisense_ /etc/munin/plugins/pisense_clock
$ sudo emacs /etc/munin/plugin-conf.d/munin-node
{% endhighlight %}

```
[pisense_*]
user root
```

{% highlight bash %}
$ munin-node-configure
$ sudo /etc/init.d/munin-node restart
{% endhighlight %}


External references
===================

- <http://pingbin.com/2011/07/how-to-install-munin-on-debian/>
- <http://raspiblog.besaba.com/?p=62>
