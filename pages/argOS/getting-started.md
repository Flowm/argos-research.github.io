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

To prepare a working SD card you can use the following shell script (similar to [eewik](https://eewiki.net/display/linuxonarm/PandaBoard#PandaBoard-SetupmicroSDcard)).

```sh
#!/bin/bash

DISK=/dev/mmcblk0

echo "zero ${DISK}"
sudo dd if=/dev/zero of=${DISK} bs=1M count=10

echo "Install bootloader"
sudo dd if=./u-boot_panda-a6/MLO of=${DISK} count=1 seek=1 bs=128k
sudo dd if=./u-boot_panda-a6/u-boot.img of=${DISK} count=2 seek=1 bs=384k

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

[Uboot Image](https://nextcloud.os.in.tum.de/s/xAxEQYA57SIhnhz)

## QEMU (PBXA9)

## Odroid U3 (TODO)
