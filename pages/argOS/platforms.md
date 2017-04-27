---
title: Platform Instructions
keywords: vagrant, development, install
last_updated: April 04, 2017
tags: [development]
summary: ""
sidebar: main_sidebar
permalink: platforms.html
folder: "argOS"
---

# General instructions

Execute the following steps:

Change `PROJECT` and `GENODE_TARGET` in [operating-system/Makefile](https://github.com/argos-research/operating-system/blob/master/Makefile)

## Generic Workflow
1. Get the prepared u-boot files (see subsection **Files**)
2. Prepare the SD card (see next section)
3. Install your project files on the SD Card

* Notes

  * `GENODE_BUILD_DIR` points to genode's building directory (e.g. `genode-focnados_panda`)
  * `PROJECT` is the name of the project (e.g. `demo`)

```sh
$> make build_dir
$> sudo make run
```
## Install Project files

* Mount your SD Card (e.g. `/dev/mmcblk0`) on `/mnt`

* Execute the following steps

```sh
$> cp $(GENODE_BUILD_DIR)/var/run/$(PROJECT)/image.elf /mnt
$> cp $(GENODE_BUILD_DIR)/var/run/$(PROJECT)/modules.list /mnt
$> cp -R $(GENODE_BUILD_DIR)/var/run/$(PROJECT)/genode /mnt
```

## Preboot Execution Environment (PXE) Configuration

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

## Setup SD-Card

To prepare a working SD card you can use the following example script (similar to [eewiki](https://eewiki.net/display/linuxonarm/PandaBoard#PandaBoard-SetupmicroSDcard)).

**We are assuming DISK=/dev/mmcblk0 as your sd card device**.

```sh
#!/bin/bash

if [[ "$1" == "--help" ]]; then
   echo "Please choose as first parameter the location of your MLO (./MLO)"
   echo "Please choose as second parameter the location of your 'u-boot.img'(./u-boot.img)"
   echo "If you dont choose any parameters the default ones(brackets) will be choosen"
   echo -e "\033[31m";
   echo "Use with care no input validation is implemented! This can damage your sd card!"
   echo -e "\033[30m";
   exit
fi

DISK=/dev/mmcblk0

echo "zero ${DISK}"
sudo dd if=/dev/zero of=${DISK} bs=1M count=10

#.MLO is now $1
echo "Install bootloader"
if [[ -z "$1" ]]; then
	sudo dd if=./MLO of=${DISK} count=1 seek=1 bs=128k
else
	sudo dd if=$1 of=${DISK} count=1 seek=1 bs=128k
fi

#./u-boot.img is now $2
if [[ -z "$2" ]]; then
	sudo dd if=./u-boot.img of=${DISK} count=2 seek=1 bs=384k
else
	sudo dd if=$2 of=${DISK} count=2 seek=1 bs=384k
fi

echo "Create Partition"
#uncomment the following lines if sfdisk <= 2.25.x
#sudo sfdisk --in-order --Linux --unit M ${DISK} <<-__EOF__
#1,,0x83,*
#__EOF__

#we're using  sfdisk >= 2.26.x
sudo sfdisk ${DISK} <<-__EOF__
1M,,0x83,*
__EOF__

echo "Format Partition"
sudo mkfs.vfat ${DISK}p1

echo "Sync"
sudo sync

echo "READY"

if [[ -z "$S1" ]]; then
  echo -e "\033[31m";
  echo "Caution: Standard parameters selected: if you did not intend to do this please check if everything is ok!"
  echo -e "\033[00m";
fi
```

# Platform Specific Instructions

## PandaBoard ES

### Select correct target device

Change `GENODE_TARGET` inside the `Makefile` to `focnados_panda`

```make
GENODE_TARGET = focnados_panda
```

### Pre-compiled files
  * [U-boot Image](https://github.com/argos-research/operating-system/tree/master/Panda%20SD)


### References
Further (deeper) information about this topic can be found under the following links:

* [MLO & u-boot](http://elinux.org/Panda_How_to_MLO_%26_u-boot)

* [uboot PandaBoard ES](http://elinux.org/PandaBoard_ES_uboot_howto)

* [Linux on ARM](https://eewiki.net/display/linuxonarm/PandaBoard)


## Raspberry PI

### Select correct target device
Change `GENODE_TARGET` inside the `Makefile` to `foc_rpi`

```make
GENODE_TARGET = foc_rpi
```

### Flash (PXE Version)
**We are assuming DISK=/dev/mmcblk0 as your sd card device**.
```sh
sudo dd if=./RaspberryPiU-Boot.img of=/dev/mmcblk0 bs=4M
```

For further PXE/tftp instructions [pxe/tftp](/pxe.html) section.

### Pre-compiled files
* [RaspberryPiU-Boot.img](https://nextcloud.os.in.tum.de/s/xAxEQYA57SIhnhz)

### References
* [Building U-Boot](http://wiki.beyondlogic.org/index.php?title=Compiling_uBoot_RaspberryPi)



## QEMU (PBXA9)
### Select correct target device

```make
GENODE_TARGET = focnados_pbxa9
```

### Building
* Execute the following steps
```sh
sudo make vde
sudo make run
```
