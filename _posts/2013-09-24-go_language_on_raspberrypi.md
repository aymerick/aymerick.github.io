---
layout: post
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

{% highlight bash %}
$ wget http://dave.cheney.net/paste/go1.1.2.linux-arm~multiarch-armv6-1.tar.gz
$ sudo tar -C /usr/local -xzf go1.1.2.linux-arm~multiarch-armv6-1.tar.gz
{% endhighlight %}

Setup
=====

{% highlight bash %}
$ emacs /etc/profile
{% endhighlight %}

Add `:/usr/local/go/bin` to `PATH` then relog.

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

- <http://dave.cheney.net/unofficial-arm-tarballs>
- <http://tip.golang.org/doc/install>
