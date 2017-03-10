---
title: Install
keywords: vagrant, development, install
last_updated: March 10, 2017
tags: [install]
summary: ""
sidebar: main_sidebar
permalink: install.html
folder: "argOS"
---

# Prerequisites

## System

* [ubuntu 16.04 lts](https://www.ubuntu.com/download/desktop)

## Packages

The following packages need to be installed (either from the corresponding
  website or via repository):

* [git](https://git-scm.com/)

* [oracle virtualbox](https://www.virtualbox.org)

* [vagrant](https://www.vagrantup.com)

# Quick Steps

The following steps bootstrap the virtual machine.
```sh
$> git clone https://github.com/argos-research/operating-system.git
$> cd operating-system
$> git submodule init
$> git submodule update
$> vagrant up
```

With the following commands we will login to the virtual machine and start the build process for our default target ([PandaBoard ES](http://pandaboard.org/)).
```sh
$> vagrant ssh
$> cd /vagrant
$> sudo make run
```

# Next Steps

* Build for other platforms

* Auxiliary files (e.g. uboot)
