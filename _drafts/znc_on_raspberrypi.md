---
layout: post
title: ZNC on Raspberry Pi
tags:
- raspberrypi
- pikan.local
---

Prerequisites
=============

- [Headless Raspberry Pi setup]({% post_url 2013-09-17-headless_raspberrypi_setup %})


Option 1: Install from sources
==============================

The raspbian package is very old, so its preferable to install it from sources:

    $ wget http://znc.in/releases/znc-1.4.tar.gz
    $ tar zxvf znc-1.4.tar.gz
    $ cd znc-1.4
    $ ./configure
    $ make

Wait.... one hour...

    $ sudo make install

Generate configuration file:

    $ znc --makeconf

    [ .. ] Checking for list of available modules...
    [ >> ] ok
    [ ** ] Building new config
    [ ** ]
    [ ** ] First let's start with some global settings...
    [ ** ]
    [ ?? ] What port would you like ZNC to listen on? (1025 to 65535): <PORT>
    [ ?? ] Would you like ZNC to listen using SSL? (yes/no) [no]: yes
    [ ?? ] Would you like ZNC to listen using both IPv4 and IPv6? (yes/no) [yes]: no
    [ .. ] Verifying the listener...
    [ >> ] ok
    [ ** ]
    [ ** ] -- Global Modules --
    [ ** ]
    [ ** ] +-----------+----------------------------------------------------------+
    [ ** ] | Name      | Description                                              |
    [ ** ] +-----------+----------------------------------------------------------+
    [ ** ] | partyline | Internal channels and queries for users connected to znc |
    [ ** ] | webadmin  | Web based administration module                          |
    [ ** ] +-----------+----------------------------------------------------------+
    [ ** ] And 10 other (uncommon) modules. You can enable those later.
    [ ** ]
    [ ?? ] Load global module <partyline>? (yes/no) [no]: no
    [ ?? ] Load global module <webadmin>? (yes/no) [no]: yes
    [ ** ]
    [ ** ] Now we need to set up a user...
    [ ** ]
    [ ?? ] Username (AlphaNumeric): <USERNAME>
    [ ?? ] Enter Password:
    [ ?? ] Confirm Password:
    [ ?? ] Would you like this user to be an admin? (yes/no) [yes]:
    [ ?? ] Nick [<USERNAME>]:
    [ ?? ] Alt Nick [<USERNAME>_]:
    [ ?? ] Ident [<USERNAME>]:
    [ ?? ] Real Name [Got ZNC?]: Rieul Nem
    [ ?? ] Bind Host (optional): 0.0.0.0
    [ ?? ] Number of lines to buffer per channel [50]: 100
    [ ?? ] Would you like to clear channel buffers after replay? (yes/no) [yes]:
    [ ?? ] Default channel modes [+stn]:
    [ ** ]
    [ ** ] -- User Modules --
    [ ** ]
    [ ** ] +--------------+------------------------------------------------------------------------------------------+
    [ ** ] | Name         | Description                                                                              |
    [ ** ] +--------------+------------------------------------------------------------------------------------------+
    [ ** ] | chansaver    | Keep config up-to-date when user joins/parts                                             |
    [ ** ] | controlpanel | Dynamic configuration through IRC. Allows editing only yourself if you're not ZNC admin. |
    [ ** ] | perform      | Keeps a list of commands to be executed when ZNC connects to IRC.                        |
    [ ** ] | webadmin     | Web based administration module                                                          |
    [ ** ] +--------------+------------------------------------------------------------------------------------------+
    [ ** ] And 21 other (uncommon) modules. You can enable those later.
    [ ** ]
    [ ?? ] Load module <chansaver>? (yes/no) [no]: yes
    [ ?? ] Load module <controlpanel>? (yes/no) [no]: yes
    [ ?? ] Load module <perform>? (yes/no) [no]: yes
    [ ?? ] Load module <webadmin>? (yes/no) [no]: yes
    [ ** ]
    [ ?? ] Would you like to set up a network? (yes/no) [no]: yes
    [ ?? ] Network (e.g. `freenode' or `efnet'): freenode
    [ ** ]
    [ ** ] -- Network Modules --
    [ ** ]
    [ ** ] +-------------+-------------------------------------------------------------------------------------------------+
    [ ** ] | Name        | Description                                                                                     |
    [ ** ] +-------------+-------------------------------------------------------------------------------------------------+
    [ ** ] | chansaver   | Keep config up-to-date when user joins/parts                                                    |
    [ ** ] | keepnick    | Keep trying for your primary nick                                                               |
    [ ** ] | kickrejoin  | Autorejoin on kick                                                                              |
    [ ** ] | nickserv    | Auths you with NickServ                                                                         |
    [ ** ] | perform     | Keeps a list of commands to be executed when ZNC connects to IRC.                               |
    [ ** ] | simple_away | This module will automatically set you away on IRC while you are disconnected from the bouncer. |
    [ ** ] +-------------+-------------------------------------------------------------------------------------------------+
    [ ** ] And 20 other (uncommon) modules. You can enable those later.
    [ ** ]
    [ ?? ] Load module <chansaver>? (yes/no) [no]: yes
    [ ?? ] Load module <keepnick>? (yes/no) [no]: yes
    [ ?? ] Load module <kickrejoin>? (yes/no) [no]: yes
    [ ?? ] Load module <nickserv>? (yes/no) [no]: yes
    [ ?? ] Load module <perform>? (yes/no) [no]: yes
    [ ?? ] Load module <simple_away>? (yes/no) [no]: yes
    [ ** ]
    [ ** ] -- IRC Servers --
    [ ** ] Only add servers from the same IRC network.
    [ ** ] If a server from the list can't be reached, another server will be used.
    [ ** ]
    [ ?? ] IRC server (host only): irc.freenode.net
    [ ?? ] [irc.freenode.net] Port (1 to 65535) [6667]:
    [ ?? ] [irc.freenode.net] Password (probably empty):
    [ ?? ] Does this server use SSL? (yes/no) [no]: no
    [ ** ]
    [ ?? ] Would you like to add another server for this IRC network? (yes/no) [no]:
    [ ** ]
    [ ** ] -- Channels --
    [ ** ]
    [ ?? ] Would you like to add a channel for ZNC to automatically join? (yes/no) [yes]:
    [ ?? ] Channel name: #fotonauts
    [ ?? ] Would you like to set up another network? (yes/no) [no]:
    [ ** ]
    [ ?? ] Would you like to set up another user? (yes/no) [no]:
    [ .. ] Writing config [/home/pi/.znc/configs/znc.conf]...
    [ >> ] ok
    [ ** ]
    [ ** ]To connect to this ZNC you need to connect to it as your IRC server
    [ ** ]using the port that you supplied.  You have to supply your login info
    [ ** ]as the IRC server password like this: user/network:pass.
    [ ** ]
    [ ** ]Try something like this in your IRC client...
    [ ** ]/server <znc_server_ip> +<PORT> <USERNAME>:<pass>
    [ ** ]And this in your browser...
    [ ** ]https://<znc_server_ip>:<PORT>/
    [ ** ]
    [ ?? ] Launch ZNC now? (yes/no) [yes]: yes
    [ .. ] Opening config [/home/pi/.znc/configs/znc.conf]...
    [ >> ] ok
    [ .. ] Loading global module [webadmin]...
    [ >> ] [/usr/local/lib/znc/webadmin.so]
    [ .. ] Binding to port [+<PORT>] using ipv4...
    [ >> ] ok
    [ ** ] Loading user [<USERNAME>]
    [ ** ] Loading network [freenode]
    [ .. ] Loading network module [chansaver]...
    [ >> ] [/usr/local/lib/znc/chansaver.so]
    [ .. ] Loading network module [keepnick]...
    [ >> ] [/usr/local/lib/znc/keepnick.so]
    [ .. ] Loading network module [kickrejoin]...
    [ >> ] [/usr/local/lib/znc/kickrejoin.so]
    [ .. ] Loading network module [nickserv]...
    [ >> ] [/usr/local/lib/znc/nickserv.so]
    [ .. ] Loading network module [perform]...
    [ >> ] [/usr/local/lib/znc/perform.so]
    [ .. ] Loading network module [simple_away]...
    [ >> ] [/usr/local/lib/znc/simple_away.so]
    [ .. ] Adding server [irc.freenode.net 6667 ]...
    [ >> ] ok
    [ .. ] Loading user module [chansaver]...
    [ >> ] [/usr/local/lib/znc/chansaver.so]
    [ .. ] Loading user module [controlpanel]...
    [ >> ] [/usr/local/lib/znc/controlpanel.so]
    [ .. ] Loading user module [perform]...
    [ >> ] [/usr/local/lib/znc/perform.so]
    [ .. ] Loading user module [webadmin]...
    [ >> ] [/usr/local/lib/znc/webadmin.so]
    [ .. ] Forking into the background...
    [ >> ] [pid: 19449]
    [ ** ] ZNC 1.4 - http://znc.in


Login to web admin: https://<IP>:<PORT>


Option 2: Install from raspbian package
=======================================

If you prefer, you can install the old raspbian package (znc 0.026):

    $ sudo apt-get install znc znc-extra


Generate configuration file:

    $ znc --makeconf

    [ ** ] Building new config
    [ ** ]
    [ ** ] First let's start with some global settings...
    [ ** ]
    [ ?? ] What port would you like ZNC to listen on? (1025 to 65535): <PORT>
    [ ?? ] Would you like ZNC to listen using SSL? (yes/no) [yes]:
    [ ?? ] Would you like ZNC to listen using ipv6? (yes/no) [yes]: no
    [ ?? ] Listen Host (Blank for all ips):
    [ ok ] Verifying the listener...
    [ ** ]
    [ ** ] -- Global Modules --
    [ ** ]
    [ ** ] +-----------+----------------------------------------------------------+
    [ ** ] | Name      | Description                                              |
    [ ** ] +-----------+----------------------------------------------------------+
    [ ** ] | partyline | Internal channels and queries for users connected to znc |
    [ ** ] | webadmin  | Web based administration module                          |
    [ ** ] +-----------+----------------------------------------------------------+
    [ ** ] And 13 other (uncommon) modules. You can enable those later.
    [ ** ]
    [ ?? ] Load global module <partyline>? (yes/no) [no]:
    [ ?? ] Load global module <webadmin>? (yes/no) [no]: yes
    [ ** ]
    [ ** ] Now we need to set up a user...
    [ ** ] ZNC needs one user per IRC network.
    [ ** ]
    [ ?? ] Username (AlphaNumeric): <USERNAME>
    [ ?? ] Enter Password:
    [ ?? ] Confirm Password:
    [ ?? ] Would you like this user to be an admin? (yes/no) [yes]: yes
    [ ?? ] Nick [<USERNAME>]: <USERNAME>
    [ ?? ] Alt Nick [<USERNAME>_]: <USERNAME>_
    [ ?? ] Ident [<USERNAME>]:
    [ ?? ] Real Name [Got ZNC?]: Rieul Nem
    [ ?? ] Bind Host (optional): 0.0.0.0
    [ ?? ] Number of lines to buffer per channel [50]: 100
    [ ?? ] Would you like to keep buffers after replay? (yes/no) [no]: no
    [ ?? ] Default channel modes [+stn]:
    [ ** ]
    [ ** ] -- User Modules --
    [ ** ]
    [ ** ] +-------------+------------------------------------------------------------------------------------------------------------+
    [ ** ] | Name        | Description                                                                                                |
    [ ** ] +-------------+------------------------------------------------------------------------------------------------------------+
    [ ** ] | admin       | Dynamic configuration of users/settings through IRC. Allows editing only yourself if you're not ZNC admin. |
    [ ** ] | chansaver   | Keep config up-to-date when user joins/parts                                                               |
    [ ** ] | keepnick    | Keep trying for your primary nick                                                                          |
    [ ** ] | kickrejoin  | Autorejoin on kick                                                                                         |
    [ ** ] | nickserv    | Auths you with NickServ                                                                                    |
    [ ** ] | perform     | Keeps a list of commands to be executed when ZNC connects to IRC.                                          |
    [ ** ] | simple_away | Auto away when last client disconnects                                                                     |
    [ ** ] +-------------+------------------------------------------------------------------------------------------------------------+
    [ ** ] And 36 other (uncommon) modules. You can enable those later.
    [ ** ]
    [ ?? ] Load module <admin>? (yes/no) [no]: yes
    [ ?? ] Load module <chansaver>? (yes/no) [no]: yes
    [ ?? ] Load module <keepnick>? (yes/no) [no]: yes
    [ ?? ] Load module <kickrejoin>? (yes/no) [no]: yes
    [ ?? ] Load module <nickserv>? (yes/no) [no]: yes
    [ ?? ] Load module <perform>? (yes/no) [no]: yes
    [ ?? ] Load module <simple_away>? (yes/no) [no]: yes
    [ ** ]
    [ ** ] -- IRC Servers --
    [ ** ] Only add servers from the same IRC network.
    [ ** ] If a server from the list can't be reached, another server will be used.
    [ ** ]
    [ ?? ] IRC server (host only): irc.freenode.net
    [ ?? ] [irc.freenode.net] Port (1 to 65535) [6667]:
    [ ?? ] [irc.freenode.net] Password (probably empty):
    [ ?? ] Does this server use SSL? (yes/no) [no]:
    [ ** ]
    [ ?? ] Would you like to add another server for this IRC network? (yes/no) [no]:
    [ ** ]
    [ ** ] -- Channels --
    [ ** ]
    [ ?? ] Would you like to add a channel for ZNC to automatically join? (yes/no) [yes]:
    [ ?? ] Channel name: #<CHANNEL>
    [ ?? ] Would you like to add another channel? (yes/no) [no]:
    [ ** ]
    [ ?? ] Would you like to set up another user (e.g. for connecting to another network)? (yes/no) [no]:
    [ ok ] Writing config [/home/pi/.znc/configs/znc.conf]...
    [ ** ]
    [ ** ] To connect to this ZNC you need to connect to it as your IRC server
    [ ** ] using the port that you supplied.  You have to supply your login info
    [ ** ] as the IRC server password like this: user:pass.
    [ ** ]
    [ ** ] Try something like this in your IRC client...
    [ ** ] /server <znc_server_ip> +<PORT> <USERNAME>:<pass>
    [ ** ] And this in your browser...
    [ ** ] https://<znc_server_ip>:<PORT>/
    [ ** ]
    [ ?? ] Launch ZNC now? (yes/no) [yes]: yes
    [ ok ] Opening Config [/home/pi/.znc/configs/znc.conf]...
    [ ok ] Loading Global Module [webadmin]... [/usr/lib/znc/webadmin.so]
    [ ok ] Binding to port [+<PORT>] using ipv4...
    [ ** ] Loading user [<USERNAME>]
    [ ok ] Adding Server [irc.freenode.net 6667 ]...
    [ ok ] Loading Module [admin]... [/usr/lib/znc/admin.so]
    [ ok ] Loading Module [chansaver]... [/usr/lib/znc/chansaver.so]
    [ ok ] Loading Module [keepnick]... [/usr/lib/znc/keepnick.so]
    [ ok ] Loading Module [kickrejoin]... [/usr/lib/znc/kickrejoin.so]
    [ ok ] Loading Module [nickserv]... [/usr/lib/znc/nickserv.so]
    [ ok ] Loading Module [perform]... [/usr/lib/znc/perform.so]
    [ ok ] Loading Module [simple_away]... [/usr/lib/znc/simple_away.so]
    [ ok ] Forking into the background... [pid: 11785]
    [ ** ] ZNC 0.206+deb2 - http://znc.in

Login to web admin: https://<IP>:<PORT>

Note: In service script, uncomment that line:

    # DAEMON=/usr/bin/znc/$NAME

And comment that line:

    DAEMON=/usr/local/bin/znc/$NAME


Add an service script
=====================

Stop ZNC:

    $ killall znc

Add Pid file to ZNC conf:

    $ emacs ~/.znc/configs/znc.conf

    PidFile = /var/run/znc/znc.pid

Add service script:

    $ sudo emacs /etc/init.d/znc

{% highlight bash %}
#! /bin/sh
### BEGIN INIT INFO
# Provides:          znc
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: ZNC IRC bouncer
# Description:       ZNC is an IRC bouncer
### END INIT INFO

PATH=/sbin:/usr/sbin:/bin:/usr/bin:/usr/local/bin
DESC="ZNC daemon"
NAME=znc
# Installed via sources:
DAEMON=/usr/local/bin/znc/$NAME
# WARNING - Use that line if installed via apt-get:
# DAEMON=/usr/bin/znc/$NAME
DAEMON_ARGS=""
PIDDIR=/var/run/znc
PIDFILE=$PIDDIR/$NAME.pid
SCRIPTNAME=/etc/init.d/$NAME
USER=pi
GROUP=pi

# Exit if the package is not installed
[ -x "$DAEMON" ] || exit 0

# Read configuration variable file if it is present
[ -r /etc/default/$NAME ] && . /etc/default/$NAME

# Load the VERBOSE setting and other rcS variables
. /lib/init/vars.sh

# Define LSB log_* functions.
# Depend on lsb-base (>= 3.2-14) to ensure that this file is present
# and status_of_proc is working.
. /lib/lsb/init-functions

#
# Function that starts the daemon/service
#
do_start()
{
    # Return
    #   0 if daemon has been started
    #   1 if daemon was already running
    #   2 if daemon could not be started
    if [ ! -d $PIDDIR ]
    then
        mkdir $PIDDIR
    fi
    chown $USER:$GROUP $PIDDIR
    start-stop-daemon --start --quiet --pidfile $PIDFILE --exec $DAEMON --test --chuid $USER > /dev/null || return 1
    start-stop-daemon --start --quiet --pidfile $PIDFILE --exec $DAEMON --chuid $USER -- $DAEMON_ARGS > /dev/null || return 2
}

#
# Function that stops the daemon/service
#
do_stop()
{
    # Return
    #   0 if daemon has been stopped
    #   1 if daemon was already stopped
    #   2 if daemon could not be stopped
    #   other if a failure occurred
    start-stop-daemon --stop --quiet --retry=TERM/30/KILL/5 --pidfile $PIDFILE --name $NAME --chuid $USER
    RETVAL="$?"
    [ "$RETVAL" = 2 ] && return 2
    # Wait for children to finish too if this is a daemon that forks
    # and if the daemon is only ever run from this initscript.
    # If the above conditions are not satisfied then add some other code
    # that waits for the process to drop all resources that could be
    # needed by services started subsequently.  A last resort is to
    # sleep for some time.
    start-stop-daemon --stop --quiet --oknodo --retry=0/30/KILL/5 --exec $DAEMON --chuid $USER
    [ "$?" = 2 ] && return 2
    # Many daemons don't delete their pidfiles when they exit.
    rm -f $PIDFILE
    return "$RETVAL"
}

#
# Function that sends a SIGHUP to the daemon/service
#
do_reload() {
    start-stop-daemon --stop --signal 1 --quiet --pidfile $PIDFILE --name $NAME --chuid $USER
    return 0
}

case "$1" in
  start)
    [ "$VERBOSE" != no ] && log_daemon_msg "Starting $DESC" "$NAME"
    do_start
    case "$?" in
        0|1) [ "$VERBOSE" != no ] && log_end_msg 0 ;;
        2) [ "$VERBOSE" != no ] && log_end_msg 1 ;;
    esac
    ;;
  stop)
    [ "$VERBOSE" != no ] && log_daemon_msg "Stopping $DESC" "$NAME"
    do_stop
    case "$?" in
        0|1) [ "$VERBOSE" != no ] && log_end_msg 0 ;;
        2) [ "$VERBOSE" != no ] && log_end_msg 1 ;;
    esac
    ;;
  status)
    status_of_proc -p $PIDFILE "$DAEMON" "$NAME" && exit 0 || exit $?
    ;;
  reload)
    log_daemon_msg "Reloading $DESC" "$NAME"
    do_reload
    log_end_msg $?
    ;;
  restart)
    log_daemon_msg "Restarting $DESC" "$NAME"
    do_stop
    case "$?" in
      0|1)
        do_start
        case "$?" in
            0) log_end_msg 0 ;;
            1) log_end_msg 1 ;; # Old process is still running
            *) log_end_msg 1 ;; # Failed to start
        esac
        ;;
      *)
        # Failed to stop
        log_end_msg 1
        ;;
    esac
    ;;
  *)
    echo "Usage: $SCRIPTNAME {status|start|stop|reload|restart}" >&2
    exit 3
    ;;
esac
{% endhighlight %}

Setup service script:

    $ sudo chmod 755 /etc/init.d/znc
    $ sudo update-rc.d znc defaults

Start ZNC:

    $ sudo service znc start
    $ sudo service znc status


Connect with colloquy
=====================

Add a new server:

  - Enable SSL
  - Disable SASL
  - Server Address: <ZNC IP>
  - Port: <ZNC PORT>
  - Server Password / Password: <ZNC PASSWORD>
  - Nickname: <ZNC USERNAME>
  - Username: <ZNC USERNAME>
  - Personal Password / Nick Pass.: <NICKSERV PASSWORD>

Connect to server.

If this error message appears on colloquy:

    <*status>    Cannot connect to IRC (Cannot assign requested address (Is your IRC server's host name valid?)). Retrying...

Type:

    /znc setbindhost 0.0.0.0
    /msg *status jump


Install push notification module
================================

NOTE: I disable that module, because it sends weirds *ghost* notifications to my iPhone.

## Option 1: If you installed ZNC from sources

    $ curl -LO http://github.com/wired/colloquypush/raw/master/znc/colloquy.cpp
    $ znc-buildmod colloquy.cpp
    $ sudo mv colloquy.so /usr/local/lib/znc/
    $ sudo service znc restart

Type on IRC:

    /msg *status loadmod colloquy
    /msg *colloquy SET attachedpush 0

    /msg *colloquy HELP

Enable "Push Notifications" on mobile Colloquy, then reconnect it to ZNC.

    /msg *colloquy LIST

## Option 2: If you installed ZNC from raspbian package

    $ sudo apt-get install znc-dev
    $ curl -LO http://github.com/wired/colloquypush/raw/0.20x/znc/colloquy.cpp
    $ znc-buildmod colloquy.cpp

@todo Configure push notification.


Log stuff
=========

Go to `https://<SERVER>:<PORT>/mods/global/webadmin/edituser` and check the **log** module (and only there).

Logs are now saved into `/home/pi/.znc/users/<USERNAME>/moddata/log`.
