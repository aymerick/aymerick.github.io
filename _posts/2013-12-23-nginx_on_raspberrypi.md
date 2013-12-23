---
layout: post
title: Nginx on a Raspberry Pi
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
$ sudo apt-get install nginx
{% endhighlight %}

Setup
=====

{% highlight bash %}
$ sudo mkdir /var/www
$ sudo chown www-data:www-data /var/www
$ sudo unlink /etc/nginx/sites-enabled/default
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
}
{% endhighlight %}

{% highlight bash %}
$ sudo emacs /var/www/index.html
{% endhighlight %}

{% highlight html %}
<html>
<head>
<title>pikan.local</title>
</head>
<body>
<h1>Welcome to pikan</h1>
</body>
</html>
{% endhighlight %}

{% highlight bash %}
$ cd /etc/nginx/sites-enabled
$ sudo ln -s ../sites-available/pikan
$ sudo service nginx start
{% endhighlight %}


Test
====

Browse:

- <http://pikan.local>
- <http://pikan.local/stats>


External references
===================

- <http://www.ducky-pond.com/posts/2013/Sep/setup-a-web-server-on-rpi/>
