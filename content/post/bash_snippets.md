+++
title = "Some bash one-liners"
date = "2025-11-10T09:48:23+13:00"
author = "Geert"
description = "Miscellaneous commands to execute various operations in bash."
draft = "false"
+++

Rename files `dot-<name>` to `.<name>` recursively (dry-run):
```bash
for file in $(find . -name 'dot-*') ; do echo mv $file $(echo $file | sed 's/dot-/./') ; done
```
Replace `do echo mv ...` with `do mv ...` to execute the rename.


