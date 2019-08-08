---
title: "Automatic Vim PlugUpdate"
date: 2019-08-04T21:58:21-07:00
draft: false
tags: ["computer", "vim"]
---

This post was inspired by this [Reddit
thread](https://www.reddit.com/r/vim/comments/clf6l2/vimplug_run_plugupdate_every_week_automatically/).

I use [vim-plug](https://github.com/junegunn/vim-plug) to handle my vim
plugins and I want to keep my plugins up-to-date. Ideally, this would be done
automatically but I don't want to suddenly have a broken vim setup. Luckily,
`vim-plug` comes with a snapshot feature.

I save the following as `~/bin/vimplug_update`.
{{< highlight bash>}}
#! /usr/bin/env bash
vim_cmd="nvim"
BACKUP_DIR="$HOME/.vimplug_snapshots"
BACKUP_FILE="$BACKUP_DIR/snapshot_$(date +"%Y-%m-%d-%H%M%S").vim"

mkdir -p "$BACKUP_DIR"

$vim_cmd -c "PlugSnapshot $BACKUP_FILE" +qa
$vim_cmd +PlugUpdate +qa
{{< /highlight >}}

Then I set a daily cron job by running `crontab -e` and writing
`@daily ~/bin/vimplug_update`. If I ever have to restore my settings, I run
```
vim -S snapshot_2019-08-05-085852.vim
```
