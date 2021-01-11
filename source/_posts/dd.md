---
title: dd
date: 2021-01-01 12:44:06
tags: [linux, shell]
description: Notes for dd syncing
cover: /img/linux.png

---
__data duplicator__
- clone one hard disk to another har disk. 
This is useful when we are building many machinese with same configuration. 
We no need to install OS on all the machines. 
Just install OS and require software on machine then clone with below example. 
```sh
dd if=/dev/sda of=/dev/sdb
```
- use gzip or bzip2 if you think image is too large
```sh
dd if=/dev/sda2 | bzip hdadisk.img.bz2
```
- take backup of a partition/complete HDD for future restoration
```sh
# backing up a partition to a file (to my home directory as hdadisk.img)
dd if=/dev/sda2 of=~/hdadisk.img
# restoring this image file in to other machine
dd if=hdadisk.img of=/dev/sdb3
```

- file copier if you don't have cp command
```sh
dd if=/home/imran/abc.txt of=/mnt/abc.txt
```

- wipe/delete content of a disk so that it will be empty for some one to use it
__this will wipe out your second hard disk and every bit is written with zero__
```sh
dd if=/dev/zero of=/dev/sdb
```