---
title: QEMU Configuration
keywords: vagrant, development, install
last_updated: April 04, 2017
tags: [development]
summary: ""
sidebar: main_sidebar
permalink: qemu.html
folder: "argOS"
---

# Building the System

The system’s build process has been fully automated for the most part. The central Make-
file is the only control element required to build, modify, and maintain the toolchain,
apart from the dependencies.

The framework’s code is hosted on ```GitHub``` as a branch of the ```702nADOS Genode```
repository:

```sh
$ git clone -b dom0 https://github.com/702nADOS/genode.git
```

This will clone the approximately 22MiB repository to a local directory.

## Dependencies
**Ubuntu Packages**

For most dependencies these packages will be required:
```sh
$ sudo apt-get install make git subversion
```
The following packages are required for building and running Genode:
```sh
$ sudo apt-get install libncurses5-dev libSDL-dev tclsh expect byacc \
genisoimage autoconf2.64 autogen bison flex g++ git gperf libxml2-utils \
xsltproc
```
Additional packages may be required depending on the Genode version and platform.
Those should be installed as requested by the build process.

## VDE
For network communication, the framework Virtual Distributed Ethernet (VDE) is
required. The revision ```r587``` from the VDE SourceForge SVN was used in testing this
toolchain [VDE].

In order to build VDE, the instructions should be followed as listed on their website:
```sh
$ svn co https://vde.svn.sourceforge.net/svnroot/vde/trunk/vde-2 vde_svn
$ cd vde_svn
$ autoreconf -fi
$ ./configure --enable-experimental
$ make
$ sudo make install
```
The commands ```vde_switch```, ```vde_tunctl```, and ```vde_plug2tap``` should now be glob-
ally available on the system.

## QEMU

QEMU is required for virtualizing the target hardware; in this case a PBX-A9 board. A
standard build of QEMU is provided by the Ubuntu repositories. This build, however,
comes without VDE support. Accordingly, a custom build of QEMU is required. This
toolchain has been tested with commit ```4b6eda626fdb8bf90472c6868d502a2ac09abeeb```
of QEMU’s git repository, but any current version will work, too:
```sh
$ git clone git://git.qemu-project.org/qemu.git
$ cd qemu
$ ./configure
```
In the output of ```configure``` VDE support should be listed with ```yes``` . Finally,
```make install``` will build and install QEMU globally.

## Genode, tms-sim, and dom0
The entire toolchain can be built by issuing a single ```make``` command [dom0]. This make
target will build most of the targets listed in Table 7.1 in order. The only targets skipped
are ```run``` which starts the showcase system, and the targets for the setup of an optional
DHCP server ```dhcp-stop``` and ```dhcp``` , as the static IP ```192.168.0.14``` is used by default.
After running ```make``` once, ```make run``` will start the dom0 TCP/IP server and task
manager in a QEMU instance.
