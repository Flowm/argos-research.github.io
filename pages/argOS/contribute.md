---                                                                                                                                                                                 
title: How to contribute
keywords: vagrant, development, install
last_updated: November 24, 2016
tags: [development]
summary: ""
sidebar: main_sidebar
permalink: contribute.html
folder: "argOS"
---

## Vagrant instructions

* Packages required:

  * vagrant
  * virtualbox

* Inside operating-system folder:

  * change "PROJECT" inside Makefile to your needs
  * adjust memory size and number of cores inside Vagrant file to fit your hardware
  * run "ip a" on your local machine and change the dev option inside bootstrap.sh to your LAN adapter
  * vagrant up (Wait until it finished. This may take some time. Machine will throw a tty error when finished, please ignore this as long as no other errors occur...)
  * run "vagrant ssh" to log into vm
  * inside vm run "cd /vagrant"
  * and run "sudo make run"
  * the compiled image will be accessible via PXE boot
  * all files for the SD card for Pandaboard to run PXE boot are inside the Panda SD folder
  * a direct LAN connection between the Pandaboard and the local machine is requiered
