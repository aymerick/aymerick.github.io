---
layout: post
kind: post
title: Setup go language workspace
tags:
- dev
- golang
---

Setup Workspace
===============

{% highlight bash %}

$ mkdir -p ~/Dev/go
$ emacs ~/.bash_profile

{% endhighlight %}

```
export GOPATH=$HOME/Dev/go
export PATH=$PATH:$GOPATH/bin
```

{% highlight bash %}

$ source ~/.bash_profile
$ echo $GOPATH

{% endhighlight %}


Fetch package
=============

{% highlight bash %}

$ go get github.com/aymerick/jeego

{% endhighlight %}

To Fetch updated package:

{% highlight bash %}

$ go get -u github.com/aymerick/jeego

{% endhighlight %}


Dev on package
==============

{% highlight bash %}

$ cd ~/Dev/go/src/github.com/aymerick/jeego/

{% endhighlight %}

Make some changes to source files.

{% highlight bash %}

$ go build
$ go install
$ jeego

{% endhighlight %}


External References
===================

- <http://tip.golang.org/doc/code.html>
