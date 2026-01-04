+++
title = "lf File Manager"
date = 2026-01-05T09:35:33+13:00
author = "Geert"
description = "How to install and use the lf file manager on Linux"
categories = ["Linux", "file manager", "TUI"]
tags = ["lf", "go", "tmux"]
draft = false
+++

# Introduction
`lf` (as in "list files") is a terminal file manager written in Go.

It is not a GUI (Graphical User Interface), but a TUI (Terminal User Interface).

Details: [lf on GitHub](https://github.com/gokcehan/lf)

# Installation
`lf` needs Go:
```bash
# install Go
sudo apt update && sudo apt install golang-go -y
# install lf
env CGO_ENABLED=0 go install -ldflags="-s -w" github.com/gokcehan/lf@latest
# check installation
~/go/bin/lf
# create an alias for convenience
alias lf="~/go/bin/lf"
# make alias permanent (if desired)
echo alias lf="~/go/bin/lf" >> ~/.bash_aliases
# start lf in current directory
lf
```

# tmux
If you use `tmux` to start `lf`, you can easily create multiple instances of `lf`:
```bash
# use tmux to start lf (C-m is literal)
tmux new \; send-keys 'lf' C-m
# create a pane to the right (C-b is control-b)
C-b %
# start lf (or any other command)
lf
# create a pane at the bottom
C-b "
# start lf (or any other command)
lf
```
Command `tmux new \; send-keys 'lf' C-m` can also be turned into an alias.

This approach has the disadvantage that it leaves you in `tmux` when you exit `lf`.

A way around this is to create an anonymous session and attach to it:
```bash
tmux new -d "~/go/bin/lf" && tmux a
```

# lfcd
`lf` has option `-print-last-dir` that prints the last directory you visited when you exit `lf`.

This can be used to change directory to the last visited directory when you exit `lf`.

Set up an alias: `alias lfcd='cd "$(~/go/bin/lf -print-last-dir "$@")"'`.

Or as a function in `~/.bashrc`:
```bash
lfcd () {
    cd "$(~/go/bin/lf -print-last-dir "$@")"
}
```
