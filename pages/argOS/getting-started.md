---
title: Getting Started
keywords: vagrant, development, install
last_updated: November 24, 2016
tags: [development]
summary: ""
sidebar: main_sidebar
permalink: getting-started.html
folder: "argOS"
---

# General instructions

### PXE

  * run "ip a" on your local machine and change the dev option inside bootstrap.sh to your LAN adapter

  * the compiled image will be accessible via PXE boot

  * all files for the SD card for Pandaboard to run PXE boot are inside the Panda SD folder

  * a direct LAN connection between the Pandaboard and the local machine is requiered

  * Setup with PXE Boot (e.g. PandaBoard ES)

Extract from `/etc/dhcp/dhcpd.conf` (See [dhcpd sample configuration](https://wiki.ubuntuusers.de/ISC-DHCPD/#Beispielkonfiguration) for furhter details)

```
host pandaboardpxe {
  hardware ethernet 02:02:01:07:16:80;
  fixed-address 192.168.0.10;
  option host-name "tuinf01-pandaboard";
  filename "pandaboard/image.elf";
  server-name "Pandaboard-PXEserv";
}
host odroid {
 hardware ethernet 00:10:75:2a:ae:e0;
 fixed-address 192.168.0.35;
 option host-name "odroid-u3";
 filename "odroid/uImage";
 server-name "Odroid-PXEserv";
}

# this is the PXE-Boot for this subnet
next-server 131.159.12.22;
filename "raspberry/genode.img";
```

### vagrant

  * adjust memory size and number of cores inside Vagrant file to fit your hardware

### genode project

  * change "PROJECT" inside Makefile to your needs

# Platform Specific Instructions

## PandaBoard ES

1. Get the prepared u-boot files (see subsection **Files**)
2. Prepare the SD card (see next section)
3. Install your project files on the SD Card

### Setup SD Card

To prepare a working SD card you can use the following example script (similar to [eewiki](https://eewiki.net/display/linuxonarm/PandaBoard#PandaBoard-SetupmicroSDcard)).

**We are assuming DISK=/dev/mmcblk0 as your sd card device**.

```sh
#!/bin/bash

DISK=/dev/mmcblk0

echo "zero ${DISK}"
sudo dd if=/dev/zero of=${DISK} bs=1M count=10

echo "Install bootloader"
sudo dd if=./u-boot_panda/MLO of=${DISK} count=1 seek=1 bs=128k
sudo dd if=./u-boot_panda/u-boot.img of=${DISK} count=2 seek=1 bs=384k

echo "Create Partition"
sudo sfdisk --in-order --Linux --unit M ${DISK} <<-__EOF__
1,,0x83,*
__EOF__

echo "Format Partition"
sudo mkfs.vfat ${DISK}p1

echo "Sync"
sudo sync

echo "READY"
```

### Install Project files

* Mount your SD Card (e.g. `/dev/mmcblk0`) on `/mnt`

* Execute the following steps

```sh
$> cp $(GENODE_BUILD_DIR)/var/run/$(PROJECT)/image.elf /mnt
$> cp $(GENODE_BUILD_DIR)/var/run/$(PROJECT)/modules.list /mnt
$> cp -R $(GENODE_BUILD_DIR)/var/run/$(PROJECT)/genode /mnt
```

* Notes

  * `GENODE_BUILD_DIR` points to genode's building directory (e.g. `genode-focnados_panda`)
  * `PROJECT` is the name of the project (e.g. `demo`)

### Howto
Further (deeper) information about this topic can be found under the following links:

* [MLO & u-boot](http://elinux.org/Panda_How_to_MLO_%26_u-boot)

* [uboot PandaBoard ES](http://elinux.org/PandaBoard_ES_uboot_howto)

* [Linux on ARM](https://eewiki.net/display/linuxonarm/PandaBoard)

### Files
* [U-boot Image](https://github.com/argos-research/operating-system/tree/master/Panda%20SD)

## Raspberry PI

* [Uboot Image](https://nextcloud.os.in.tum.de/s/xAxEQYA57SIhnhz)

* [Building U-Boot](http://wiki.beyondlogic.org/index.php?title=Compiling_uBoot_RaspberryPi)

## QEMU (PBXA9)

## Odroid U3 (TODO)

* [Building U-Boot](http://odroid.com/dokuwiki/doku.php?id=en:u3_building_u-boot)

* [Hardware C1](http://odroid.com/dokuwiki/doku.php?id=en:c1_hardware)

### Troubleshoot

* http://forum.odroid.com/viewtopic.php?f=82&t=9128
* https://github.com/mkaczanowski/u-boot/tree/odroid-u3-usbnet
* http://stackoverflow.com/questions/21256866/libz-so-1-cannot-open-shared-object-file
* http://stackoverflow.com/questions/34418659/arm-eabi-gcc-no-such-file-or-directory-in-cyanogenmod-source
