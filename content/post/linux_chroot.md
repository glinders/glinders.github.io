+++
title = "Linux chroot"
date = 2025-12-09T07:57:36+13:00
author = "Geert"
description = "Set up a chroot environment"
categories = ["Linux"]
tags = ["chroot", "ldd", ]
draft = true
+++
# what is chroot
`chroot` stands for "change root". This command changes the root directory (/) for a process and its children.

After calling chroot, the process sees the specified directory as its root filesystem. It cannot access files outside that directory, because `/` now points to the new location.

It is commonly used for:
- create a sandbox or `chroot jail` to securely run services
- to build or test software in an isolated environment
- create a recovery environments (e.g., fix a broken system from a live CD)

# chroot example
Steps to create a chroot environment for a command. Use `cat` as an example:
```bash
# find out which libraries we need
ldd $(which cat)
  linux-vdso.so.1 (0x00007ffe0b17f000)
  libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x000077f4f6e00000)
  /lib64/ld-linux-x86-64.so.2 (0x000077f4f71ab000)
# create directories
mkdir -p cat_chroot/{bin,lib64,lib/x86_64-linux-gnu}
# copy cat command
cp $(which cat) cat_chroot/bin/
# copy required libraries
cp /lib/x86_64-linux-gnu/libc.so.6 cat_chroot/lib/x86_64-linux-gnu/
cp /lib64/ld-linux-x86-64.so.2 cat_chroot/lib64/
# run cat command (must be run as root)
sudo chroot cat_chroot/ /bin/cat --version
  cat (GNU coreutils) 9.4
```

# ldd
For a more detailed description of `ldd` see [ldd and readelf]({{< ref "post/linux_ldd_readelf.md" >}})
