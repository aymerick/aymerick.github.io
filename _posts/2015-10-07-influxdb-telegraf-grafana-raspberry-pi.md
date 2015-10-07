---
layout: post
kind: post
title: InfluxDB, Telegraf and Grafana on Raspberry Pi
tags:
- raspberrypi
---

# Prerequisites

- [Go language on Raspberry Pi]({% post_url 2013-09-24-go_language_on_raspberrypi %})

# InfluxDB

## Install dependencies

{% highlight bash %}
$ sudo apt-get install ruby-dev gcc
$ sudo gem install fpm
{% endhighlight %}

## Build InfluxDB package

Let's build InfluxDB 0.9.4 from sources:

{% highlight bash %}
$ gvm pkgset create influxdb
$ gvm pkgset use influxdb
$ mkdir -p ~/.gvm/pkgsets/go1.5/influxdb/src/github.com/influxdb
$ cd ~/.gvm/pkgsets/go1.5/influxdb/src/github.com/influxdb
$ git clone https://github.com/influxdb/influxdb.git
$ cd influxdb
$ ./package.sh -t deb -p 0.9.4
{% endhighlight %}

## Install InfluxDB

Install package:

{% highlight bash %}
$ sudo dpkg -i influxdb_0.9.4_armhf.deb
{% endhighlight %}

Start server:

{% highlight bash %}
$ sudo service influxdb start
{% endhighlight %}

Test server:

{% highlight bash %}
$ /opt/influxdb/influx
{% endhighlight %}

Configuration file is located at: `/etc/opt/influxdb/influxdb.conf`

# Telegraf

## Install dependencies

{% highlight bash %}
$ sudo apt-get install rpm
{% endhighlight %}

## Build Telegraf package

Let's build Telegraf 0.1.9 from sources:

{% highlight bash %}
$ gvm pkgset create telegraf
$ gvm pkgset use telegraf
$ go get github.com/tools/godep
$ mkdir -p ~/.gvm/pkgsets/go1.5/telegraf/src/github.com/influxdb
$ cd ~/.gvm/pkgsets/go1.5/telegraf/src/github.com/influxdb
$ git clone https://github.com/influxdb/telegraf.git
$ cd telegraf
$ ./scripts/package.sh
{% endhighlight %}

    ...
    Commence creation of unknown packages, version 0.1.9-27-gb4e8a23? [Y/n] Y
    ...
    Publish packages to S3? [y/N] N
    ...

## Install Telegraf

Install package:

{% highlight bash %}
$ sudo dpkg -i telegraf_0.1.9-27-gb4e8a23_armhf.deb
{% endhighlight %}

Start server:

{% highlight bash %}
$ sudo service telegraf start
{% endhighlight %}

Configuration file is located at: `/etc/opt/telegraf/telegraf.conf`

# Grafana

## Install dependencies

{% highlight bash %}
$ sudo apt-get install ruby-dev rpm
$ sudo gem install fpm
{% endhighlight %}

## Install nodejs

Install pre-built binaries:

{% highlight bash %}
$ sudo mkdir /opt/node
$ wget https://nodejs.org/dist/v4.1.2/node-v4.1.2-linux-armv7l.tar.gz
$ tar zxvf node-v4.1.2-linux-armv7l.tar.gz
$ sudo cp -r node-v4.1.2-linux-armv7l/* /opt/node
$ rm node-v4.1.2-linux-armv7l.tar.gz
{% endhighlight %}

Adds those lines to `/etc/profile` juste before `export PATH`:

    NODE_JS_HOME="/opt/node"
    PATH="$PATH:$NODE_JS_HOME/bin"

##  Install Grafana

Let's build Grafana 2.1.3 from scratch.

Setup:

{% highlight bash %}
$ gvm pkgset create grafana
$ gvm pkgset use grafana
$ go get github.com/grafana/grafana
$ cd ~/.gvm/pkgsets/go1.5/grafana/src/github.com/grafana/grafana/
$ git checkout tags/v2.1.3
$ go run build.go setup
$ godep restore
$ npm install
$ sudo /opt/node/bin/npm install -g grunt-cli
{% endhighlight %}

Change `build.go` file, line 76, with `grunt("--force","release")` to bypass an issue with `phantomjs`.

Cf. <https://github.com/grafana/grafana/issues/2683>.

Fixes a timeout issue:

{% highlight bash %}
$ sed -i 's/baseUrl: '\''.\/app'\'',/baseUrl: '\''.\/app'\'',waitSeconds: 0,/' tasks/options/requirejs.js
{% endhighlight %}

Build package:

{% highlight bash %}
$ go run build.go build package
{% endhighlight %}

Install package:

{% highlight bash %}
$ sudo dpkg -i dist/grafana_2.1.3_armhf.deb
{% endhighlight %}

Setup service:

{% highlight bash %}
$ sudo update-rc.d grafana-server defaults 95 10
{% endhighlight %}

Start server:

{% highlight bash %}
$ sudo service grafana-server start
{% endhighlight %}

Configuration files are located at: `/etc/default/grafana-server` and `/etc/grafana/grafana.ini`.

Navigate with a browser to: <http://RPI_IP:3000/>

Authenticate with `admin` / `admin` credendials then change password at: <http://RPI_IP:3000/profile/password>.

Add the telegraf influxdb datasource as explained at: <http://docs.grafana.org/datasources/influxdb/>:

    - Name: RPI-NAME
    - Default: checked
    - Type: InfluxDB 0.9.x
    - Url: http://localhost:8086/
    - Access: proxy
    - Basic Auth: NOT checked
    - Database: telegraf
    - User: admin
    - Password: admin

You can now create cool `grafana` dashboards. Enjoy.

# External References

- <https://nicolas.steinmetz.fr/blog/post/InfluxDB-0.9.3-sur-un-RaspberriPy>
- <http://docs.grafana.org/project/building_from_source/>
- <https://github.com/fg2it/grafana-on-raspberry/blob/master/v2.1.2%2FREADME.md>
