+++
title = "Git Snippets"
date = 2026-02-24T14:58:24+13:00
author = "Geert"
description = "Some miscellaneous git commands on Linux"
categories = ["Version Control"]
tags = ["git", ]
draft = false
+++

# Remove a submodule

Remove a Git submodule:
- deinitialise the submodule
- remove its configuration and local files
- commit & push changes

## deinitialise submodule

This command removes the submodule's section from the `.git/config` file and
removes the working tree.
```bash
# remove submodule (use -f to force)
$ git submodule deinit [-f] <path/to/submodule>
```

## remove from .gitmodules

This command removes the entry from the .gitmodules file.
```bash
# remove from .gitmodules
$ git rm <path/to/submodule>
```

## delete the submodule's git directory

A submodule's git repository data is kept in the main project's `.git/modules` directory.
If you want to be able to check out past commits that contain this submodule, then leave this in place.
For a complete removal, you can delete this directory manually:
```bash
$ rm -rf .git/modules/<submodule>
```

## finalise changes

To make the change permanent, commit & push them.
```bash
# commit & push
$ git commit -m "Removed submodule <submodule_name>"
$ git push
```

# Change the URL of a submodule
The URL is stored in file `.git/config` and in file `.gitmodules`.
You can change it manually or use the matching command:
```bash
# change URL of a submodule
$ git submodule set-url <submodule> <new-url>

# sync changes
$ git submodule sync <submodule>
```

