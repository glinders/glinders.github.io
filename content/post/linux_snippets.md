+++
title = "Some Linux snippets"
date = 2025-11-27T09:19:53+13:00
author = "Geert"
description = "Miscellaneous bits of Linux knowledge"
categories = ["Linux"]
tags = ["sudo", "newgrp", ]
draft = false
+++

# extend sudo timeout & scope
Create a new file in `/etc/sudoers.d/`:
```bash
visudo /etc/sudoers.d/glinders-timeout
```
add lines:
```
# extend my sudo session timeout & share across terminals
Defaults:glinders  timestamp_timeout=240, !tty_tickets
```
This sets the timeout to 4 hours and extends the scope to all terminals.

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

