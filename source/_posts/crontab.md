---
title: crontab
date: 2021-01-01 12:41:46
tags: [linux, shell]
cover: /img/linux.png

---
```sh
# $ crontab -e 
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │                                       7 is also Sunday on some systems)
# │ │ │ │ │  
# │ │ │ │ │
# * * * * *  command_to_execute

# For capture image every minutes when I get home
* 0-9，18-23 * * *

# Empty temp folder every friday at 5pm
0 5 * 5 rm -rf /tmp/*

# back up images to google drive every night at midnight
0 0 * * * rsync -a ~/Pictures/ ~/Google\ drive/Pictures/

```