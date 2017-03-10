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

- SD card

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

## Raspberry PI

## QEMU (PBXA9)

## Odroid U3 (TODO)
