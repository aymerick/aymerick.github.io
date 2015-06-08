---
layout: post
title: Transmission on Raspberry Pi
tags:
- raspberrypi
- pickup.local
---

Prerequisites
=============

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})


Install
=======

    $ sudo apt-get install transmission-daemon

    $ sudo mkdir /mnt/usbdrive/torrents
    $ sudo mkdir /mnt/usbdrive/torrents/completed
    $ sudo chmod a+w /mnt/usbdrive/torrents/completed
    $ sudo mkdir /mnt/usbdrive/torrents/downloading
    $ sudo chmod a+w /mnt/usbdrive/torrents/downloading

Fix conf:

    $ sudo emacs /etc/transmission-daemon/settings.json

Set:

    "download-dir": "/mnt/usbdrive/torrents/completed",
    "incomplete-dir": "/mnt/usbdrive/torrents/downloading",
    "incomplete-dir-enabled": true,
    "rpc-whitelist": "127.0.0.1,192.168.0.*",
    "rpc-port": [PORT],

Reload conf:

    $ sudo invoke-rc.d transmission-daemon reload

Visit <http://pickup.local:[PORT]> with transmission/transmission credentials
