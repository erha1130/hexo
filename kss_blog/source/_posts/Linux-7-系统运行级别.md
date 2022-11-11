---
title: Linux-7-系统运行级别
date: 2022-10-06 14:26:06
tags: linux
categories: linux
---
# 系统运行级别
## Linux运行级别（centos6）
运行级别
![linux40.png](https://s2.loli.net/2022/10/06/X5Tl2avPOiqHADh.png)
查看默认级别：vim /etc/inittab  
Linux系统有7种运行级别（runlevel）：常用的是级别3和5  
* 运行级别0:系统停机状态，系统默认运行级别不能设为0，否则不能正常启动
* 运行级别1:单用户工作状态，root权限，用于系统维护，禁止远程登陆
* 运行级别2:多用户状态(没有NFS)，不支持网络
* 运行级别3:完全的多用户状态(有NFS)，登陆后进入控制台命令行模式
* 运行级别4:系统未使用，保留
* 运行级别5:X11控制台，登陆后进入图形GUl模式
* 运行级别6:系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动

## CentOS7的运行级别简化为：
multi-user.target 等价于原运行级别3（多用户有网，无图形界面)  
graphical.target等价于原运行级别5(多用户有网，有图形界面)
## 查看当前运行级别:
systemctl get-default
## 修改当前运行级别
* systemctl set-default TARGET.target(这里TARGET取multi-user或者 graphical)  
* init 3 or 5(centos6)