+++
title = "ldd and readelf"
date = 2025-12-11T08:49:31+13:00
author = "Geert"
description = "What are ldd and readelf"
categories = ["Linux"]
tags = ["ldd", "readelf", ]
draft = true
+++


# what is ldd
The `ldd` command shows the shared libraries required by an (ELF) executable or shared object (a `.so` file).


# what is readelf

```bash
readelf -d /lib/x86_64-linux-gnu/libc.so.6|egrep '(NEEDED|SONAME)'
  0x0000000000000001 (NEEDED)             Shared library: [ld-linux-x86-64.so.2]
  0x000000000000000e (SONAME)             Library soname: [libc.so.6]
```
