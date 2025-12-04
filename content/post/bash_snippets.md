+++
title = "Some bash one-liners"
date = "2025-11-10T09:48:23+13:00"
author = "Geert"
description = "Miscellaneous commands to execute various operations in bash."
categories = ["bash"]
tags = ["rename"]
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