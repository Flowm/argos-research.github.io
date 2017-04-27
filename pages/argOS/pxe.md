---
title: PXE Configuration
keywords: vagrant, development, install
last_updated: April 04, 2017
tags: [development]
summary: ""
sidebar: main_sidebar
permalink: pxe.html
folder: "argOS"
---

# Preboot Execution Environment (PXE) Configuration

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
