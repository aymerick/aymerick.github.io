---
layout: post
kind: post
title: Go language on Raspberry Pi
tags:
- raspberrypi
- dev
- golang
---

Prerequisites
=============

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})

Install
=======

_Note that you can install pre-built packages here: <http://dave.cheney.net/unofficial-arm-tarballs>_

Install go 1.5 using Go Version Manager:

{% highlight bash %}
$ bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
$ source /home/pi/.gvm/scripts/gvm
$ sudo apt-get install bison
$ gvm install go1.4
$ gvm use go1.4
$ gvm install go1.5
$ gvm use go1.5 --default
{% endhighlight %}

Test
====

Create a file named `hello.go` and put the following code in it:

{% highlight go %}
package main

import "fmt"

func main() {
    fmt.Printf("hello, world\n")
}
{% endhighlight %}

Then run it with the go tool:

{% highlight bash %}
$ go run hello.go
{% endhighlight %}


External References
===================

- <https://nicolas.steinmetz.fr/blog/post/InfluxDB-0.9.3-sur-un-RaspberriPy>
- <https://github.com/moovweb/gvm>
- <http://dave.cheney.net/unofficial-arm-tarballs>
- <http://tip.golang.org/doc/install>
