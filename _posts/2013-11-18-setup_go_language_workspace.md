---
layout: post
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


Fetch external package
======================

{% highlight bash %}

$ go get github.com/tarm/goserial
$ ls ~/Dev/go/src/github.com/tarm/

{% endhighlight %}


External References
===================

- <http://tip.golang.org/doc/code.html>
