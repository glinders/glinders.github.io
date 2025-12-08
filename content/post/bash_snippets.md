+++
title = "Some bash one-liners"
date = "2025-11-10T09:48:23+13:00"
author = "Geert"
description = "Miscellaneous commands to execute various operations in bash."
categories = ["bash"]
tags = ["rename", "groups", "hash"]
draft = false
+++

# bulk rename files
Rename files `dot-<name>` to `.<name>` recursively (dry-run):
```bash
for file in $(find . -name 'dot-*') ; do echo mv $file $(echo $file | sed 's/dot-/./') ; done
```
Replace `do echo mv ...` with `do mv ...` to execute the rename.

# add group without logout/login
When adding a user to a group, you have to log out and then log back in to make the change take effect. This can however also be done immediately using `newgrp`:
```bash
# create new group
sudo groupadd <new-group>
# add user to group
sudo usermod -aG $USER
# take effect immediately
newgrp <new-group>
```

# update lookup cache
In bash, the hash keyword is a builtin command used to manage the shell’s internal command lookup cache. This cache stores the full path of executables you’ve already run, so bash doesn’t have to search through $PATH every time you call them. If the executable moves or $PATH changes, the cache can become stale.
```bash
$ sudo apt  install hugo
...
$ which hugo
/usr/bin/hugo
$ hugo version
hugo v0.123.7+extended linux/amd64
$ sudo apt purge hugo
...
$ sudo dpkg -i hugo_0.150.0_linux-amd64.deb
...
```
In a different terminal:
```bash
$ which hugo
/usr/local/bin/hugo
$ hugo version
bash: /usr/bin/hugo: No such file or directory
$ hash -d hugo
$ hugo version
hugo v0.150.0-3f5473b7d4e7377e807290c3acc89feeef1aaa71 linux/amd64
```