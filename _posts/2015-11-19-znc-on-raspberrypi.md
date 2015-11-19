---
layout: post
title: ZNC IRC bouncer on Raspberry Pi
tags:
- raspberrypi
---

Prerequisites
=============

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})


Install from sources
====================

The raspbian package is very old, so its preferable to install it from sources:

    $ wget http://znc.in/releases/znc-1.6.2.tar.gz
    $ tar zxvf znc-1.6.2.tar.gz
    $ cd znc-1.6.2
    $ sudo apt-get install build-essential libssl-dev libperl-dev pkg-config
    $ ./configure
    $ make

Wait.... 30 minutes on a rpi2... one hour on a rpi1...

    $ sudo make install

Generate configuration file:

    $ znc --makeconf

    [ .. ] Checking for list of available modules...
    [ >> ] ok
    [ ** ]
    [ ** ] -- Global settings --
    [ ** ]
    [ ?? ] Listen on port (1025 to 65534): <PORT>
    [ ?? ] Listen using SSL (yes/no) [no]: yes
    [ ?? ] Listen using both IPv4 and IPv6 (yes/no) [yes]: yes
    [ .. ] Verifying the listener...
    [ >> ] ok
    [ ** ] Unable to locate pem file: [/home/pi/.znc/znc.pem], creating it
    [ .. ] Writing Pem file [/home/pi/.znc/znc.pem]...
    [ >> ] ok
    [ ** ] Enabled global modules [webadmin]
    [ ** ]
    [ ** ] -- Admin user settings --
    [ ** ]
    [ ?? ] Username (alphanumeric): aymerick
    [ ?? ] Enter password:
    [ ?? ] Confirm password:
    [ ?? ] Nick [aymerick]:
    [ ?? ] Alternate nick [aymerick_]:
    [ ?? ] Ident [aymerick]:
    [ ?? ] Real name [Got ZNC?]: Rieul Nem
    [ ?? ] Bind host (optional): 0.0.0.0
    [ ** ] Enabled user modules [chansaver, controlpanel]
    [ ** ]
    [ ?? ] Set up a network? (yes/no) [yes]: yes
    [ ** ]
    [ ** ] -- Network settings --
    [ ** ]
    [ ?? ] Name [freenode]:
    [ ?? ] Server host [chat.freenode.net]:
    [ ?? ] Server uses SSL? (yes/no) [yes]:
    [ ?? ] Server port (1 to 65535) [6697]:
    [ ?? ] Server password (probably empty):
    [ ?? ] Initial channels: #test
    [ ** ] Enabled network modules [simple_away]
    [ ** ]
    [ .. ] Writing config [/home/pi/.znc/configs/znc.conf]...
    [ >> ] ok
    [ ** ]
    [ ** ] To connect to this ZNC you need to connect to it as your IRC server
    [ ** ] using the port that you supplied.  You have to supply your login info
    [ ** ] as the IRC server password like this: user/network:pass.
    [ ** ]
    [ ** ] Try something like this in your IRC client...
    [ ** ] /server <znc_server_ip> +48914 aymerick:<pass>
    [ ** ]
    [ ** ] To manage settings, users and networks, point your web browser to
    [ ** ] https://<znc_server_ip>:48914/
    [ ** ]
    [ ?? ] Launch ZNC now? (yes/no) [yes]: yes
    [ .. ] Opening config [/home/pi/.znc/configs/znc.conf]...
    [ >> ] ok
    [ .. ] Loading global module [webadmin]...
    [ >> ] [/usr/local/lib/znc/webadmin.so]
    [ .. ] Binding to port [+48914]...
    [ >> ] ok
    [ ** ] Loading user [aymerick]
    [ ** ] Loading network [freenode]
    [ .. ] Loading network module [simple_away]...
    [ >> ] [/usr/local/lib/znc/simple_away.so]
    [ .. ] Adding server [chat.freenode.net +6697 ]...
    [ >> ] ok
    [ .. ] Loading user module [chansaver]...
    [ >> ] ok
    [ .. ] Loading user module [controlpanel]...
    [ >> ] ok
    [ .. ] Forking into the background...
    [ >> ] [pid: 17547]
    [ ** ] ZNC 1.6.2 - http://znc.in


Login to web admin: https://<IP>:<PORT>


Start znc on server start
=========================

Edit crontab:

    $ crontab -e

Add:

    */5 * * * * /usr/local/bin/znc >/dev/null 2>&1


Log stuff
=========

Go to `https://<SERVER>:<PORT>/mods/global/webadmin/edituser` and check the **log** module (and only there).

Logs are now saved into `/home/pi/.znc/users/<USERNAME>/moddata/log`.
