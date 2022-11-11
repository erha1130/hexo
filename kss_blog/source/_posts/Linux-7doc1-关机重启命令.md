---
title: Linux-7doc1-关机重启命令
date: 2022-10-06 15:31:21
tags: linux
categories: linux
---
## 基本语法
1. sync
(功能描述:将数据由内存同步到硬盘中)
2. halt
(功能描述:停机，关闭系统，但不断电>
3. poweroff
(功能描述:关机，断电)
4. reboot
(功能描述:就是重启，等同于shutdown -r now)
5. shutdown[选项]时间
![linux41.png](https://s2.loli.net/2022/10/06/gdF1pKLSxZeyV5N.png)
   ```dotnetcli
   shutdown         //默认1分钟后关机
   shutdown -c      //取消关机
   shutdown 3       //在3分钟之后关机
   shutdown 15:28   //在15:28关机
   shutdown now     //立刻关机
   ```
   
```dotnetcli
Shut down the system.

     --help      Show this help
  -H --halt      Halt the machine
  -P --poweroff  Power-off the machine
  -r --reboot    Reboot the machine
  -h             Equivalent to --poweroff,verridden by --halt(相当于-poweroff，被-halt覆盖)
  -k             Don't halt/power-off/reboot, just send warnings
     --no-wall   Don't send wall message before halt/power-off/reboot
```
## 经验技巧
Linux 系统中为了提高磁盘的读写效率，对磁盘采取了 “预读迟写”操作方式。当用户
保存文件时，Linux 核心并不一定立即将保存数据写入物理磁盘中，而是将数据保存在缓
冲区中，等缓冲区满时再写入磁盘，这种方式可以极大的提高磁盘写入数据的效率。但是，
也带来了安全隐患，如果数据还未写入磁盘时，系统掉电或者其他严重问题出现，则将导
致数据丢失。使用 sync 指令可以立即将缓冲区的数据写入磁盘。
