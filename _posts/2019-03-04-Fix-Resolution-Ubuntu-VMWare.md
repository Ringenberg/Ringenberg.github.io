---
layout: post
title: "Simple hack for dealing with Ubuntu virtual machine under VMware Workstation not using dynamic resolution"
author: Michael Ringenberg
date: 2019-03-04
---

I have an issue with a fresh Ubuntu 18.10 install as a virtual machine in
VMware Workstation 15.0.2 where upon boot and login, the graphical display
does not adjust to the dimensions of the VMware window as it is supposed to.
I have found that the way to fix this is to run the following in a terminal
after logging in:
```systemctl restart open-vm-tools```
As this needs to be run as superuser and after the UI is running, I have not
found the appropriate place to do this or what script order needs to be
adjusted in order to fix this permanently. However, I am not too bothered by
having to run this whenever I have to restart my virtual machine.
