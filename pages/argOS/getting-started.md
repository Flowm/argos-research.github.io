---
title: Getting Started (obsolete)
keywords: vagrant, development, install
last_updated: November 24, 2016
tags: [development]
summary: ""
sidebar: main_sidebar
permalink: getting-started.html
folder: "argOS"
---







## vagrant

  * adjust memory size and number of cores inside Vagrant file to fit your hardware

## genode project

  * change "PROJECT" inside Makefile to your needs

# Platform Specific Instructions

## Raspberry PI

Download the raspberry pi sd card image for tftp/pxe boot from here:
https://nextcloud.os.in.tum.de/s/xAxEQYA57SIhnhz

and flash it to the sd card with:
sudo dd if=./RaspberryPiU-Boot.img of=/dev/mmcblk0 bs=4M

Assuming that mmcblk0 is your sd card.

For further instructions of pxe/tftp boot see pxe/tftp section.


## QEMU (PBXA9)

## Odroid U3 (broken/abandoned)

 Disclamer: Odroid U3 is not yet ready to run our project!

 The tftp boot is not working right now as genode image is not loaded correctly.
 As we cannot fix the issue we abandoned the hardware for now and concentrate on other working hardware.

 If you want to fix this issue consider reading here:

* [Building U-Boot](http://odroid.com/dokuwiki/doku.php?id=en:u3_building_u-boot)

* [Boot via tftp](http://forum.odroid.com/viewtopic.php?f=82&t=9128)

* [U3 ubuntu release](http://odroid.com/dokuwiki/doku.php?id=en:u3_release_linux_ubuntu)

* http://forum.odroid.com/viewtopic.php?f=82&t=9128
* https://github.com/mkaczanowski/u-boot/tree/odroid-u3-usbnet
* http://stackoverflow.com/questions/21256866/libz-so-1-cannot-open-shared-object-file
* http://stackoverflow.com/questions/34418659/arm-eabi-gcc-no-such-file-or-directory-in-cyanogenmod-source

Further reading and explanation can be found if you take a look at our effort on the genode mailing list starting from here:
* https://sourceforge.net/p/genode/mailman/genode-main/?viewmonth=201701&viewday=25

