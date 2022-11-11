---
title: Linux-6-系统管理
date: 2022-10-05 21:58:15
tags: linux
categories: linux
---
### linux中的进程和服务
计算机中，一个正在执行的程序或命令，被叫做“进程”（process）。  
启动之后一直存在、常驻内存的进程，一般被称作“服务”（service）。    
notice:守护进程（英语：daemon，/ˈdiːmən/或/ˈdeɪmən/）[2]是一种在后台执行，而不由用户直接交互控制的电脑程序。此类程序会被以进程的形式初始化。守护进程程序的名称通常以字母d结尾，以指明这个进程实际是守护进程，并与普通的电脑程序区分开来。例如，syslogd就是指管理系统日志的守护进程，sshd是接收传入SSH连接的守护进程。
### service服务管理(centos6版本)
#### 1.基本语法
```dotnetcli
service 服务名 start | stop | restart | status
```
#### 2.经验技巧
查看服务的方法：
```dotnetcli
# 转到此路径下
cd /etc/init.d/
# 查看所有服务
ls -al
```
高亮的为正在运行的服务
![linux39.png](https://s2.loli.net/2022/10/05/6slTOGVyYF2Cfxz.png)
#### 3.案例实操
1. 查看网络服务的状态  
[root@hadoop100 桌面]#service network status 
1. 停止网络服务  
[root@hadoop100 桌面]#service network stop
1. 启动网络服务  
[root@hadoop100 桌面]#service network start
1. 重启网络服务  
[root@hadoop100 桌面]#service network restart

### chkconfig设置后台服务的自启配置
#### 1.基本语法
chkconfig （功能描述：查看所有服务器自启配置）  
chkconfig 服务名 off （功能描述：关掉指定服务的自动启动）  
chkconfig 服务名 on （功能描述：开启指定服务的自动启动）  
chkconfig 服务名 --list （功能描述：查看服务开机启动状态）  
#### 2.开启/关闭network服务指定级别的自动启动
```dotnetcli
[root@hadoop100 桌面]#chkconfig --level 指定级别 network on
[root@hadoop100 桌面]#chkconfig --level 指定级别 network off
```
### systemctl(centos7版本)
#### 1.基本语法
systemctl    start | stop | restart | status    服务名
#### 2.经验技巧
查看服务的方法：/usr/lib/systemd/system
```dotnetcli
[root@hadoop100 system]# pwd
/usr/lib/systemd/system
[root@hadoop100 init.d]# ls -al
-rw-r--r--. 1 root root 275 4 月 27 2018 abrt-ccpp.service
......
```
#### 3.案例实操
（1）查看防火墙服务的状态  
[root@hadoop100 桌面]# systemctl status firewalld  
（2）停止防火墙服务  
[root@hadoop100 桌面]# systemctl stop firewalld  
（3）启动防火墙服务  
[root@hadoop100 桌面]# systemctl start firewalld  
（4）重启防火墙服务  
[root@hadoop100 桌面]# systemctl restart firewalld  
###  systemctl 设置后台服务的自启配置
#### 1.基本语法
systemctl list-unit-files （功能描述：查看服务开机启动状态）  
systemctl disable service_name （功能描述：关掉指定服务的自动启动）  
systemctl enable service_name （功能描述：开启指定服务的自动启动）  
#### 2.案例实操
（1）开启/关闭 iptables(防火墙)服务的自动启动  
```
[root@hadoop100 桌面]# systemctl enable firewalld.service
[root@hadoop100 桌面]# systemctl disable 
```