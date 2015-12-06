---
layout: post
title: Jeenode
---

@todo Alors attention, le **protocole** ce n'est pas un proctologue qui a mangé de la confiture, non, c'est simplement la façon dont deux nodes doivent communiquer pour se comprendre.

@todo Dit dont Ginette, il est pas un peu gros le nouveau thermomètre la ?

@todo http://jeelabs.net/projects/cafe/wiki/Dive_Into_JeeNodes


Jeelink
=======

= On Mac:
1. install FTDI drivers:
(upto 10.7) http://www.ftdichip.com/Drivers/VCP.htm
(10.8) http://www.silabs.com/products/mcu/pages/usbtouartbridgevcpdrivers.aspx

2. Update Arduino SDK to 1.0.3, and launch it

3. Plug JeeLink

4. Arduino SDK:
- select Tools / Serial Port => ???
- select Tools / Serial Monitor
  - Set 57600 baud
  - See [RF12demo.7] A i1 g212 @ 868 MHz
  - Commands:
    1l # turn activity led on
    0l # turn activity led off

Set collector mode with command: 1c (won't ack any packet, just sniffing)


Jeenode
=======

References:
- http://www.bot-thoughts.com/2013/01/getting-started-with-jeenode-v6.html
- https://github.com/jcw/jeelib/tree/master/examples/RF12/RF12demo

= On Mac:
$ cd ~/Documents/Arduino/libraries/
$ git clone https://github.com/jcw/jeelib.git

- Open Arduino and then select the RF12demo from File > Examples > jeelib > RF12 > RF12Demo
- Upload sketch
- select Tools / Serial Monitor
  - Set 57600 baud
  - See [RF12demo.7] A i1 g212 @ 868 MHz

# set node id: 2
2i

# set frequency: 868 MHz
8b

# set network group: 212
212g

# send packet with Ack to JeeLink
1a
# OK 129


Jeelink on pikan.local
======================

References:
- http://jeelabs.org/2013/02/14/dijn-07-attach-a-jeelink-to-the-rpi/
- http://geekgrandad.wordpress.com/2012/12/31/raspberry-pi-with-jeelink/

# Plug Jeelink on pikan.local

screen /dev/ttyUSB0 57600
# To quit: CTRL+A followed by “\”


Jeenode on pikan.local FTDI
===========================

References:
- http://jeelabs.org/2012/09/20/serial-hookup-jeenode-to-raspberry-pi/
- https://www.raspberrypi.org/forums/viewtopic.php?t=123081&p=828879

$ sudo usermod -a -G tty pi

# Relog

$ sudo systemctl stop serial-getty@ttyAMA0.service
$ sudo systemctl disable serial-getty@ttyAMA0.service

$ sudo emacs /boot/cmdline.txt

# Remove text: console=ttyAMA0,115200

$ sudo reboot

# Plug Jeenode on pikan.local

# Test with:
$ screen /dev/ttyAMA0 57600
# To quit: CTRL+A followed by “k”
# To detach: CTRL+A followed by “d”
# To reattach: screen -x

# OR test with:
$ sudo apt-get install minicom
$ minicom -b 57600 -o -D /dev/ttyAMA0
# To quit: CTRL+A followed by "x"


Install latest node.js on pikan.local
=====================================

References:
- http://oskarhane.com/raspberry-pi-install-node-js-and-npm/

sudo mkdir /opt/node
wget http://nodejs.org/dist/v0.10.5/node-v0.10.5-linux-arm-pi.tar.gz
tar xvzf node-v0.10.5-linux-arm-pi.tar.gz
sudo cp -r node-v0.10.5-linux-arm-pi/* /opt/node

cd ~
emacs .bash_profile

# if [ -f "$HOME/.bashrc" ] ; then
# 	source $HOME/.bashrc
# fi
#
# if [ -d "$HOME/bin" ] ; then
# 	pathman $HOME/bin last
# fi
#
# PATH=$PATH:/opt/node/bin
# export PATH

node -v
npm -v


HouseMon on pikan.local
=======================

References:
- https://github.com/jcw/housemon
- http://jeelabs.org/2013/02/15/dijn-08-set-up-node-js-and-redis/
- http://jeelabs.org/2013/02/16/dijn-09-install-the-housemon-server/

sudo apt-get install redis-server git
sudo usermod -aG staff pi && sudo reboot

git clone https://github.com/jcw/housemon.git
cd housemon
npm install

emacs briqs/emacs nodeMap-local.coffee

# Set file:
  exports.rf12nodes =
    212:
      2: 'roomNode'

  exports.rf12devices =
    'ttyUSB0':
      recvid: 1
      group: 212
      band: 868

  exports.locations =
    'RF12:212:2': title: 'bureau Aymerick'

screen
npm start

# Go to: http://pikan.local:3333/

# In Admin, enable briqs:
# - drivers
# - jcw-readings
# - jcw-staticdata
# - status
# - rf12demo:/dev/ttyUSB0
# - archiver
# - history
# - graphs

# @todo Install RDD briq to export to rddtools : http://jeelabs.net/boards/9/topics/1882


Setup RoomNode on JeeNode v6
============================

References:
- https://github.com/jcw/jeelib/blob/master/examples/RF12/roomNode/roomNode.ino
- http://jeelabs.net/projects/hardware/wiki/Room_Board
- http://jeelabs.org/2010/02/02/room-board-assembly/
- http://jeelabs.org/2010/10/20/new-roomnode-code/
- http://jeelabs.org/2010/10/21/reporting-motion/
- http://jeelabs.org/2010/11/28/meet-the-new-room-board-v2/
- http://forum.jeelabs.net/node/710

cd ~/Documents/Arduino/libraries/jeelib
git pull

# Open roomNode.ino sketch
# Upload sketch to JeeNode v6

@todo Add modified roomNode sketch to github !


ATTiny84A
=========

References:
- http://forum.arduino.cc/index.php?topic=150870.0
