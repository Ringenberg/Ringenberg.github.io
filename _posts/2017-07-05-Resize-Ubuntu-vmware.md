---
layout: post
title: "Resizing an Ubuntu virtual machine under VMware Workstation"
author: Michael Ringenberg
date: 2017-07-05
---

Having had trouble finding the instructions I needed to resize the "disk" of 
my Ubuntu based virtual machine under VMware Workstation 12, I have decided
to create my own little guide.

Environment and tools:
* VMware® Workstation 12 Pro 12.5.7
* Ubuntu 17.04 (upgraded repeatedly from way too long ago)
* gparted-0.27 (need to update)

Things to note are that my virtual machine is my main development environment
that I have been maintaining for years now and I find that I need to give it
a little extra room now and again. One of the main changes I made a while ago
is to use lvm with a single volume group and only two logical groups: one for
swap and one for everything else. In my opinion, it is more of a hindrance to
have separate partitions for virtual machines as the major reasons for having
multiple partitions do not really apply to virtual machines.

Steps:
1. While the virtual machine is shut down, use VMware's "Edit virtual machine settings" to open the settings menu.
  1. Click on the hard drive and then the "Expand" button.
  1. Increase the size to the new desired size.
  1. For the CD/DVD, specify the gparted iso and make sure "connect at powerup" is enabled.
1. Boot the virtual machine with gparted.
   1. "deactivate" the partition so that it can be edited.
   1. resize the extended partition to fill the space.
   1. resize the sub-partition to fill the space.
   1. Apply the changes.
1. Reboot into the OS.
   1. `vgdisplay` to see volume group information. Note the "Free PE /Size" value.
   1. `lvdisplay` to get the LV Path.
   1. `lvextend -L +<size>G <lv path>` to resize the logical volume.
   1. `resize2fs <lv path>` to get the file system to adjust to the new size.
