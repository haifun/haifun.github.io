---
layout: post
title:  "linux下查看磁盘空间"
date:   2014-08-04 10:03:52
categories: Linux
---

如果要查看磁盘还剩多少空间，当然是用df的命令了。

  [root@localhost ~]# df -h 

查看当前文件夹下的磁盘使用情况

  [root@localhost ~]# du --max-depth=1 -h 
  
加个排序

  [root@localhost ~]# du --max-depth=1 -h | sort -rn
