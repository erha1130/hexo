---
title: 基本的SQL语句

date: 2022-10-10 22:06:14

tags: MySQL

categories: MySQL
---

# 基本的select语句

## SQL背景

SQL（Structured Query Language，结构化查询语言）是使用关系模型的数据库应用语言， 与数据直 接打交道 ，由 IBM 上世纪70年代开发出来。后由美国国家标准局（ANSI）开始着手制定SQL标准， 先后有 SQL-86 ， SQL-89 ， SQL-92 ， SQL-99 等标准。

![linux43.png](https://s2.loli.net/2022/10/11/aRFztnuyKjG7Bh1.png)

## SQL分类

* DDL（Data Definition Languages、数据定义语言），这些语句定义了不同的数据库、表、视图、索 引等数据库对象，还可以用来创建、删除、修改数据库和数据表的结构。
  * 关键字：create、drop、alter、rename、truncate

 * DML（Data Manipulation Language、数据操作语言），用于添加、删除、更新和查询数据库记 录，并检查数据完整性。 增删改查
   * 关键字： insert、delete、update、select
 * DCL （Data Control Language、数据控制语言）、用于定义数据库、表、字段、用户的访问权限和 安全级别。
   * 关键字： grant、revoke、commit、rollback、savepoint

> **因为查询语句使用的非常的频繁，所以很多人把查询语句单拎出来一类：DQL（数据查询语言）。 还有单独将 COMMIT 、 ROLLBACK 取出来称为TCL （Transaction Control Language，事务控制语 言）。**

## SQL语言的规则与规范

* SQL 可以写在一行或者多行。为了提高可读性，各子句分行写，必要时使用缩进
* 每条命令以 ; 或 \g 或 \G 结束
* 关键字不能被缩写也不能分行

* 关于标点符号
  * 必须使用英文状态下的半角输入方式
  * 字符串型和日期时间类型的数据可以使用单引号（' '）表示
  * 列的别名，尽量使用双引号（" "），而且不建议省略as

### SQL大小写规范 （建议遵守）

* **MySQL 在 Windows 环境下是大小写不敏感的**
* **MySQL 在 Linux 环境下是大小写敏感的**
  * 数据库名、表名、表的别名、变量名是严格区分大小写的
  * 关键字、函数名、列名（或字段名）、列的别名（字段的别名）是忽略大小写的。
* **推荐采用统一的书写规范：**
  * 数据库名、表名、表别名、字段名、字段别名都小写
  * SQL关键字、函数名、绑定变量都大写

##  注释

* 单行注释： #注释文字（Mysql特有的方式）
* 单行注释：-- 注释文字（--后面必须包含一个空格。）
* 多行注释：/* 注释文字 */

## 数据导入指令

导入数据库文件，后缀为.sql

```sql
mysql> source d:\mysqldb.sql
```

```sql
mysql> desc employees;
+----------------+-------------+------+-----+---------+-------+
| Field          | Type        | Null | Key | Default | Extra |
+----------------+-------------+------+-----+---------+-------+
| employee_id    | int         | NO   | PRI | 0       |       |
| first_name     | varchar(20) | YES  |     | NULL    |       |
| last_name      | varchar(25) | NO   |     | NULL    |       |
| email          | varchar(25) | NO   | UNI | NULL    |       |
| phone_number   | varchar(20) | YES  |     | NULL    |       |
| hire_date      | date        | NO   |     | NULL    |       |
| job_id         | varchar(10) | NO   | MUL | NULL    |       |
| salary         | double(8,2) | YES  |     | NULL    |       |
| commission_pct | double(2,2) | YES  |     | NULL    |       |
| manager_id     | int         | YES  | MUL | NULL    |       |
| department_id  | int         | YES  | MUL | NULL    |       |
+----------------+-------------+------+-----+---------+-------+
11 rows in set (0.01 sec)
```

## 基本的select语句

### select...

```sql
select 1；#没有任何子句

select 9/2；#
```

### select ... from ...

* 语法

  ```SQL
  select 标识选择哪些列 from 标识从那个表中选择;
  ```

* 选择全部列

  ```SQL
  select  *  from  departments;
  ```

  > **一般情况下，除非需要使用表中所有的字段数据，最好不要使用通配符‘*’。使用通配符虽然可以节 省输入查询语句的时间，但是获取不需要的列数据通常会降低查询和所使用的应用程序的效率。通 配符的优势是，当不知道所需要的列的名称时，可以通过它获取它们。 在生产环境下，不推荐你直接使用 SELECT * 进行查询。**

* 选择特定的列

  ```SQL
  SELECT department_id, location_id
  FROM departments;
  ```

  > **MySQL中的SQL语句是不区分大小写的，因此SELECT和select的作用是相同的，但是，许多开发人 员习惯将关键字大写.**

### 列的别名

* 相当于列的小名

* 可以在列名和别名之间加入关键字**as**，也可以省略

* 别名可以使用双引号，也可以省略

* example

  ```SQL
  SELECT last_name AS name, commission_pct comm
  FROM employees;
  ```

  ```SQL
  SELECT last_name "Name", salary*12 "Annual Salary"
  FROM employees;
  ```

  **查询后显示使用别名的列**

### 去除重复行

* **默认情况下，查询会返回全部行，包括重复行。在SELECT语句中使用关键字DISTINCT去除重复行**

```SQL
SELECT DISTINCT department_id
FROM employees;
```

* **对于**

```SQL
SELECT DISTINCT department_id,salary
FROM employees;
```

​	**DISTINCT 其实是对后面所有列名的组合进行去重**

### 空值参与运算

#### 所有运算符或列值遇到null值，运算的结果都为null

```SQL
SELECT employee_id,salary,commission_pct,
12 * salary * (1 + commission_pct) "annual_sal"
FROM employees;
```

**在 MySQL 里面， 空值不等于空字符串。一个空字符串的长度是 0，而一个空值的长 度是空。而且，在 MySQL 里面，空值是占用空间的。**

#### 使用 ifnull(字段，0) 函数参与运算

```sql
SELECT employee_id,salary,commission_pct,
12 * salary * (1 + ifnull(commission_pct,0)) "annual_sal"
FROM employees;
```

### 着重号

* **错误的**

  ```SQL
  mysql> select * from order;
  ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'order' at line 1
  ```

  **原因：order 为保留字(关键字)**

  ```SQL
  mysql> select * from 'order';
  ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''order'' at line 1
  ```

  **原因：不能是 '   必须是 `**  

* **正确的**

  ```sql
  mysql> select * from `order`;
  +----------+------------+
  | order_id | order_name |
  +----------+------------+
  |        1 | shkstart   |
  |        2 | tomcat     |
  |        3 | dubbo      |
  +----------+------------+
  3 rows in set (0.00 sec)
  ```

* **结论**

  **我们需要保证表中的字段、表名等没有和保留字、数据库系统或常用方法冲突。如果真的相同，请在 SQL语句中使用一对``（着重号）引起来。**

### 查询函数

**select查询可以对常数进行查询。在select查询结果中增加一列固定的常数列。这列的取值是我们指定的，而不是从数据表中动态取出的。**

**SQL 中的 SELECT 语法的确提供了这个功能，一般来说我们只从一个表中查询数据，通常不需要增加一个 固定的常数列，但如果我们想整合不同的数据源，用常数列作为这个表的标记，就需要查询常数。**

```sql
mysql> select '尚硅谷' as corporation,last_name from employees;
+-------------+-------------+
| corporation | last_name   |
+-------------+-------------+
| 尚硅谷      | King        |
| 尚硅谷      | Kochhar     |
| 尚硅谷      | De Haan     |
| 尚硅谷      | Hunold      |
```

### 显示表结构

**使用describe或desc命令，表示表结构。**

```sql
mysql> describe employees;
+----------------+-------------+------+-----+---------+-------+
| Field          | Type        | Null | Key | Default | Extra |
+----------------+-------------+------+-----+---------+-------+
| employee_id    | int         | NO   | PRI | 0       |       |
| first_name     | varchar(20) | YES  |     | NULL    |       |
| last_name      | varchar(25) | NO   |     | NULL    |       |
| email          | varchar(25) | NO   | UNI | NULL    |       |
| phone_number   | varchar(20) | YES  |     | NULL    |       |
| hire_date      | date        | NO   |     | NULL    |       |
| job_id         | varchar(10) | NO   | MUL | NULL    |       |
| salary         | double(8,2) | YES  |     | NULL    |       |
| commission_pct | double(2,2) | YES  |     | NULL    |       |
| manager_id     | int         | YES  | MUL | NULL    |       |
| department_id  | int         | YES  | MUL | NULL    |       |
+----------------+-------------+------+-----+---------+-------+
11 rows in set (0.00 sec)
```

**各个字段的含义**

* **Field：表示字段名称。**
* **Type：表示字段类型。**
* **Null：表示该列是否可以存储NULL值。**
* **Key：表示该列是否已编制索引。PRI表示该列是表主键的一部分；UNI表示该列是UNIQUE索引的一部分；MUL表示在列中某个给定值允许出现多次。**
* **Default：表示该列是否有默认值，如果有，那么值是多少。**
* **Extra：表示可以获取的与给定列有关的附件信息，例如AUTO_INCREMENT等。**

### 过滤数据

#### 语法

```sql
select 字段1,字段2
from 表名
where 过滤条件
```

* 使用where子句，将不满足条件的行过滤掉
* where子句紧随from子句

#### 举例

```sql
mysql> select employee_id , last_name,job_id,department_id
    -> from employees 
    -> where department_id = 90;
+-------------+-----------+---------+---------------+
| employee_id | last_name | job_id  | department_id |
+-------------+-----------+---------+---------------+
|         100 | King      | AD_PRES |            90 |
|         101 | Kochhar   | AD_VP   |            90 |
|         102 | De Haan   | AD_VP   |            90 |
+-------------+-----------+---------+---------------+
3 rows in set (0.00 sec)
```

