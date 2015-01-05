---
layout: post
kind: post
title: Munin Client on a raspberry pi
tags:
- raspberrypi
---

Prerequisites
=============

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})
- [Munin Server on a Raspberry Pi]({% post_url 2013-12-24-munin_server_on_raspberrypi %})


Install
=======

{% highlight bash %}
$ sudo apt-get install munin-node munin-plugins-extra
{% endhighlight %}


Setup Node
==========

{% highlight bash %}
$ sudo emacs /etc/munin/munin-node.conf
{% endhighlight %}

```
allow ^192\.168\.0\.44$
```

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

Setup Server
============

On Munin Server (pikan.local):

{% highlight bash %}
$ sudo emacs /etc/munin/munin.conf
{% endhighlight %}

```
[NODE-NAME.local]
    address 192.168.0.42
    use_node_name yes
```

{% highlight bash %}
$ sudo service munin restart
{% endhighlight %}

Test:

{% highlight bash %}
pi@pikan:~$ telnet NODE-NAME.local 4949
Trying 192.168.0.42...
Connected to 192.168.0.42.
Escape character is '^]'.
# munin node at NODE-NAME
list
cpu df df_inode entropy forks fw_packets if_err_eth0 if_eth0 interrupts irqstats load memory nfs4_client nfs_client nfsd nfsd4 ntp_kernel_err ntp_kernel_pll_freq ntp_kernel_pll_off ntp_offset open_files open_inodes pisense_clock pisense_temp proc_pri processes swap threads uptime users vmstat
quit
Connection closed by foreign host.
pi@pikan:~$ telnet 192.168.0.42 4949
Trying 192.168.0.42...
Connected to 192.168.0.42.
Escape character is '^]'.
# munin node at NODE-NAME
nodes
NODE-NAME
.
list
cpu df df_inode entropy forks fw_packets if_err_eth0 if_eth0 interrupts irqstats load memory nfs4_client nfs_client nfsd nfsd4 ntp_kernel_err ntp_kernel_pll_freq ntp_kernel_pll_off ntp_offset open_files open_inodes pisense_clock pisense_temp proc_pri processes swap threads uptime users vmstat
fetch pisense_temp
temp.value 45.5
.
quit
Connection closed by foreign host.
{% endhighlight %}


External references
===================

- <http://pingbin.com/2012/07/howto-install-munin-raspberry-pi/>
