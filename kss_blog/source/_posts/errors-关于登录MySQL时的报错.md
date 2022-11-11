---
title: 关于登录MySQL时的报错
date: 2022-10-27 20:46:41
tags: errors
categories: errors
---

## 报文

```shell
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
```

## 解决

1. **排除是否是未开启MySQL服务的原因**
2. **进my.cnf配置文件添加skip-grant-tables**

```shell
[root@erha ~]# cat /etc/my.cnf
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

**我的my.cnf文件不在etc下的my.cnf中，里面包含的了一个路径，其实就在这个路径下(/etc/my.cnf.d)**

```shell
[root@erha ~]# ls /etc/my.cnf.d/
client.cnf  mysql-default-authentication-plugin.cnf  mysql-server.cnf
```

**里面有三个配置文件,进入mysql-server.cnf文件中**

```shell
[root@erha ~]# cat /etc/my.cnf.d/mysql-server.cnf
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
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
log-error=/var/log/mysql/mysqld.log
pid-file=/run/mysqld/mysqld.pid
skip-grant-tables
```

**在[mysqld]下添加"skip-grant-tables"**,**保存后退出**。

3. **更改root账户的密码**

   1. **无密码登录MySQL**

   ```shell
   [root@erha my.cnf.d]# mysql -uroot -p
   Enter password: 
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 13
   Server version: 8.0.26 Source distribution
   
   Copyright (c) 2000, 2021, Oracle and/or its affiliates.
   
   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   ```

   2. **修改密码**

   ```shell
   mysql> update mysql.user set authentication_string='abcdefg' where user='root';
   Query OK, 1 row affected (0.01 sec)
   ```

   3. **刷新权限**

   ```shell
   mysql> flush privileges;
   Query OK, 0 rows affected (0.01 sec)
   ```

   			4. **退出**

   ```shell
   mysql> exit
   Bye
   ```

4. **重启mysql**

```shell
[root@erha my.cnf.d]# systemctl restart mysqld
```

​		





