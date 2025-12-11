+++
title = "U-Boot Stuff"
date = 2025-11-27T12:34:57+13:00
author = "Geert"
description = "Miscellaneous bits of U-Boot knowledge."
categories = ["Linux", "U-Boot]
tags = ["U-Boot", ]
draft = false
+++

# Environment
When in Linux:
```bash
# print all environment variables
$ fw_printenv
bootcmd=bootp; setenv bootargs root=/dev/nfs nfsroot=${serverip}:${rootpath} ip=${ipaddr}:${serverip}:${gatewayip}:${netmask}:${hostname}::off; bootm
baudrate=115200
arch=arm
cpu=armv7
...
# set environment variable
$ fw_setenv bootdelay 5
# get environment variable(s)
$ fw_printenv bootdelay baudrate
bootdelay=5
baudrate=115200
```

If you get `Warning: Bad CRC, using default environment` at the start of the output, then the environment has never been initialised. As soon as you set a variable, the warning will go away.

When in U-Boot, use `printenv` instead of `fw_printenv` and `setenv` instead of `fw_setenv`.

Any `$<name>` variables should be escaped with `'` or `"` in Linux and in U-Boot.

# Reboot
Quick reboot from Linux:
```bash
sync && reboot -f
```

# Boot Delay
Environment variable `bootdelay` allows a user to interrupt the U-Boot boot process.
| `bootdelay` | comment |
|-------------|---------|
| 0           | boot immediately without waiting |
| -1          | don't boot, but enter boot shell |
| \<n>         | wait \<n> seconds for user input, if no key is pressed, run `bootcmd` |

If `fw_setenv` doesn't accept negative values:
```bash
$ fw_setenv bootdelay -1
fw_setenv: invalid option -- '1'
```
then use:
```bash
$ echo 'bootdelay -1' > tmpfile
$ fw_setenv -s tmpfile
$ rm tmpfile
$ fw_printenv bootdelay
bootdelay=-1
```