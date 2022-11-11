---
title: 关于在centos系统下开启MySQL服务时的报错解决
date: 2022-10-21 17:17:24
tags: errors
categories: errors
---
## 报错
```shell
[root@iZj6c1drl65inbvxsn0vj5Z /]# systemctl start mysqld
Job for mysqld.service failed because the control process exited with error code.
See "systemctl status mysqld.service" and "journalctl -xe" for details.
```
## 解决

### 方法一

1. **先去/etc/my.cnf文件中找到你的mysql目录**

**发现没有配置文件，里面只有一个路径**

```powershell
[root@iZj6c1drl65inbvxsn0vj5Z /]# cat /etc/my.cnf
#
# This group is read both both by the client and the server
# use it for options that affect everything
#
[client-server]

#
# include all files from the config directory
#
!includedir /etc/my.cnf.d
```
**其实配置文件就在这个路径下 **   
2. **转到这个路径下**

```powershell
[root@iZj6c1drl65inbvxsn0vj5Z etc]# cd my.cnf.d
[root@iZj6c1drl65inbvxsn0vj5Z my.cnf.d]# ls
client.cnf  mysql-default-authentication-plugin.cnf  mysql-server.cnf
```
**发现有三个配置文件 **   
3. **打开mysql-server.cnf配置文件**
```powershell
[root@iZj6c1drl65inbvxsn0vj5Z my.cnf.d]# cat mysql-server.cnf
#
# This group are read by MySQL server.
# Use it for options that only the server (but not clients) should see
#
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/en/server-configuration-defaults.html

# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mysqld according to the
# instructions in http://fedoraproject.org/wiki/Systemd

[mysqld]
datadir=/var/lib/mysql  #mysql的目录
socket=/var/lib/mysql/mysql.sock
log-error=/var/log/mysql/mysqld.log
pid-file=/run/mysqld/mysqld.pid
```
**在文件中找到mysql的目录**

4. **进入/var/lib/目录，给mysql整个777权限**  
```powershell
[root@iZj6c1drl65inbvxsn0vj5Z my.cnf.d]# cd /var/lib/
[root@iZj6c1drl65inbvxsn0vj5Z lib]# chmod 777 mysql
```
5. **删掉/var/lib/mysql/目录下所有内容**

```powershell
[root@iZj6c1drl65inbvxsn0vj5Z lib]# rm -rf /var/lib/mysql/*
```

6. **启动mysql**
```powershell
[root@iZj6c1drl65inbvxsn0vj5Z lib]# systemctl status mysqld
● mysqld.service - MySQL 8.0 database server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; disabled; vendor preset: disabled)
   Active: active (running) since Tue 2022-10-25 15:22:02 CST; 14s ago
  Process: 63886 ExecStopPost=/usr/libexec/mysql-wait-stop (code=exited, status=0/SUCCESS)
  Process: 64082 ExecStartPost=/usr/libexec/mysql-check-upgrade (code=exited, status=0/SUCCESS)
  Process: 63957 ExecStartPre=/usr/libexec/mysql-prepare-db-dir mysqld.service (code=exited, status=0/SU>  Process: 63933 ExecStartPre=/usr/libexec/mysql-check-socket (code=exited, status=0/SUCCESS)
 Main PID: 64038 (mysqld)
   Status: "Server is operational"
    Tasks: 38 (limit: 5036)
   Memory: 405.1M
   CGroup: /system.slice/mysqld.service
           └─64038 /usr/libexec/mysqld --basedir=/usr
```

### 方法二

**用vim打开mysql-server.cnf时报以下错误导致上述错误**

```shell
E325: ATTENTION Found a swap file by the name
```

* **用" ls -a"找出mysql-server.cnf.swap文件**
* **用"rm -f mysql-server.cnf.swap"删除它**
* **重启MySQL服务**
