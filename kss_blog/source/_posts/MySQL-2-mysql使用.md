---
title: 基本的增删改查
date: 2022-09-12 08:44:31
tags: MySQL
categories: MySQL
---
## 1.查看所有的数据库
```dotnetcli
show databases;
```
“information_schema”是 MySQL 系统自带的数据库，主要保存 MySQL 数据库服务器的系统信息，
比如数据库的名称、数据表的名称、字段名称、存取权限、数据文件 所在的文件夹和系统使用的
文件夹，等等
“performance_schema”是 MySQL 系统自带的数据库，可以用来监控 MySQL 的各类性能指标。
“sys”数据库是 MySQL 系统自带的数据库，主要作用是以一种更容易被理解的方式展示 MySQL 数据
库服务器的各类性能指标，帮助系统管理员和开发人员监控 MySQL 的技术性能。
“mysql”数据库保存了 MySQL 数据库服务器运行时需要的系统信息，比如数据文件夹、当前使用的
字符集、约束检查信息，等等
## 2.创建自己的数据库
```dotnetcli
create database 数据库名;
```
## 3.使用自己的数据库
```dotnetcli
use 数据库名;
```
使用完use语句之后，如果接下来的SQL都是针对一个数据库操作的，那就不用重复use了，如果要针对另一个数据库操作，那么要重新use。
## 4.查看某个库的所有表格
```dotnetcli
show tables; #要求前面有use语句
show tables from 数据库名;
```
## 5.创建新的表格
```dotnetcli
create table 表名称(字段名 数据类型,字段名 数据类型);
```
```dotnetcli
#创建学生表
create table student(id int,name varchar(20)); #说名字最长不超过20个字符
```
## 6.查看一个表的数据
```dotnetcli
select * from 数据库表名称;
```
```example
#查看学生表的数据
select * from student;
```
## 7、添加一条记录
```dotnetcli
insert into 表名称 values(值列表);
#添加两条记录到student表中
insert into student values(1,'张三');
insert into student values(2,'李四');
```
## 8.查看表的创建信息
```dotnetcli
show create table 表名称\G
#查看student表的详细创建信息
show create table student\G
#结果如下
*************************** 1. row ***************************
Table: student
Create Table: CREATE TABLE `student` (
`id` int(11) DEFAULT NULL,
`name` varchar(20) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1
1 row in set (0.00 sec)
```
上面的结果显示student的表格的默认字符集是“latin1”不支持中文。
## 9、查看数据库的创建信息
```dotnetcli
show create database 数据库名\G
#查看atguigudb数据库的详细创建信息
show create database atguigudb\G
```
```dotnetcli
#结果如下
*************************** 1. row ***************************
Database: atguigudb
Create Database: CREATE DATABASE `atguigudb` /*!40100 DEFAULT CHARACTER SET latin1 */
1 row in set (0.00 sec)
```
上面的结果显示atguigudb数据库也不支持中文，字符集默认是latin1。
## 10.删除表格
```dotnetcli
drop table 表名称;
#删除学生表
drop table student;
```
## 11.删除数据库
```dotnetcli
drop database 数据库名;
#删除atguigudb数据库
drop database atguigudb;
```