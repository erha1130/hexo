---
title: MySQL的编码设置
date: 2022-09-14 16:36:21
tags: MySQL
categories: MySQL
---
## MySQL5.7中
### 问题再现：命令行操作sql乱码问题
```dotnetcli
mysql> INSERT INTO t_stu VALUES(1,'张三','男');
ERROR 1366 (HY000): Incorrect string value: '\xD5\xC5\xC8\xFD' for column 'sname' at
row 1
```
### 问题解决
步骤1：查看编码命令
```dotnetcli
show variables like 'character_%';
show variables like 'collation_%';
```
步骤2：修改mysql的数据目录下的my.ini配置文件
```dotnetcli
[mysql] #大概在63行左右，在其下添加
...
default-character-set=utf8 #默认字符集
[mysqld] # 大概在76行左右，在其下添加
...
character-set-server=utf8
collation-server=utf8_general_ci
```
步骤3：重启服务
```dotnetcli
# 启动 MySQL 服务命令：
net start MySQL服务名
# 停止 MySQL 服务命令：
net stop MySQL服务名
```
```dotnetcli
    d:\kss_blog>net start mysql57
    发生系统错误 5。

    拒绝访问。
```
原因：不是以管理员身份打开终端
步骤4：查看编码命令
```dotnetcli
mysql> show variables like 'character_%'
    -> ;
+--------------------------+-----------------------------+
| Variable_name            | Value                       |
+--------------------------+-----------------------------+
| character_set_client     | gbk                         |
| character_set_connection | gbk                         |
| character_set_database   | utf8                        |
| character_set_filesystem | binary                      |
| character_set_results    | gbk                         |
| character_set_server     | utf8                        |
| character_set_system     | utf8                        |
| character_sets_dir       | D:\MySQL5.7\share\charsets\ |
+--------------------------+-----------------------------+
8 rows in set, 1 warning (0.00 sec)

mysql> show variables like 'collation_%';
+----------------------+-----------------+
| Variable_name        | Value           |
+----------------------+-----------------+
| collation_connection | gbk_chinese_ci  |
| collation_database   | utf8_general_ci |
| collation_server     | utf8_general_ci |
+----------------------+-----------------+
3 rows in set, 1 warning (0.00 sec)
```
## MySQL8.0中
在MySQL 8.0版本之前，默认字符集为latin1，utf8字符集指向的是utf8mb3。网站开发人员在数据库设计
的时候往往会将编码修改为utf8字符集。如果遗忘修改默认的编码，就会出现乱码的问题。从MySQL 8.0
开始，数据库的默认编码改为 utf8mb4 ，从而避免了上述的乱码问题。