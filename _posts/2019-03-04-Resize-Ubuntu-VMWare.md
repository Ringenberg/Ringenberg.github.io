---
layout: post
title: "Resizing an Ubuntu virtual machine under VMware Workstation (update)"
author: Michael Ringenberg
date: 2019-03-04
---

Some things have changed for the better regarding resizing the disk for 
an Ubuntu virtual machine under VMWare and it is much easier now.

Environment and tools:
* VMware® Workstation Pro 15.0.2
* Ubuntu 18.10 (Fresh installation a.k.a not an upgraded 18.04)
* gparted-0.33

Steps:
1. While the virtual machine is shut down, use VMware's "Edit virtual machine settings" to open the settings menu.
  1. Click on the hard drive and then the "Expand" button.
  1. Increase the size to the new desired size.
  1. For the CD/DVD, specify the gparted iso and make sure "connect at powerup" is enabled.
1. Boot the virtual machine with gparted. This might require using "Power on to Firmware" in order for the iso to boot before the OS.
   1. resize the partition to fill the space.
1. Reboot into the OS.
1. DONE! It seems that everything is smart enough to adjust to the newly available size.
