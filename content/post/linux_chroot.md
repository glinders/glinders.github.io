+++
title = "Linux_chroot"
date = 2025-12-09T07:57:36+13:00
author = "Geert"
description = "set up a chroot environment"
categories = ["Linux"]
tags = ["chroot"]
draft = false
+++

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

```bash
readelf -d /lib/x86_64-linux-gnu/libc.so.6|egrep  '(NEEDED|SONAME)'
  0x0000000000000001 (NEEDED)             Shared library: [ld-linux-x86-64.so.2]
  0x000000000000000e (SONAME)             Library soname: [libc.so.6]
```
